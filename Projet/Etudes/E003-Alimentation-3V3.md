# E003 — Architecture d'alimentation commune 3,3 V

**Date : 2026-11-10 — Hypothèse d'architecture, non testée.**

## Décision proposée
Alimenter MCU ESP32-C3, multiplexeur I²C TCA9548A, pilotes DRV2605L et capteurs sur **un rail régulé commun 3,3 V**. Les sorties pilotes vers les moteurs restent paramétrées selon leurs tensions nominales et limites de surcommande, non nécessairement en continu à 3,3 V.

## Batterie ≠ 3,3 V constant
Une cellule LiPo 1S a une tension variable (pleine ~4,2 V ; nominale ~3,7 V ; décharge minimale selon protection et fiche cellule). **Jamais de LiPo 1S brute directement sur l'alimentation 3V3 de l'ESP32-C3.** Utiliser protection batterie et convertisseur buck-boost réglé à 3,3 V ; chargeur LiPo compatible USB 5 V, sans supposer que la sortie du chargeur est du 3,3 V.

## Plages de tension
- ESP32-C3 : 3,0–3,6 V, 3,3 V recommandé ; courant d'alimentation conseillé >=500 mA pour la puce/module seule. Source : https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c3/schematic-checklist.html
- DRV2605L : alimentation 2,0–5,2 V ; réglages RATED_VOLTAGE et OD_CLAMP à calibrer pour chaque ERM. Source : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- PMD 310-118.001 : 3,0 V nominal, max. 3,3 V ; démarrage 120 mA maximum (datasheet locale au dépôt).
- NFP-FLAT-C1030 : 3,0 V nominal, 2,3–3,6 V plage ; démarrage 120 mA maximum (datasheet locale au dépôt).
- TCA9548A : 1,65–5,5 V (vérifier module et pull-up choisis). Source : https://www.ti.com/product/TCA9548A

## Courants (enveloppes préliminaires)
- Banc 2 moteurs : 2 × 120 mA pointe moteur + courant ESP32/BLE (pics potentiellement bien au-delà de sa moyenne) + pilotes, pertes et marge ; **viser >=1 A continu sur le régulateur, à vérifier par calcul de puissance et tests**.
- Gant 5 moteurs : 5 × 120 mA pointe moteur + ESP32 et autres charges ; dimensionnement à réviser potentiellement 1,5–2 A pour marge de pointe.
- Le courant **d'entrée batterie** varie avec la tension LiPo et le rendement du convertisseur, et n'est pas égal au courant du bus 3,3 V.

## Particularités des cartes de développement
Ne pas présumer que toute carte ESP32 « USB-C » accepte un 3,3 V externe sur n'importe quelle broche. Vérifier le schéma de la **référence exacte de carte**, son entrée 3V3, son régulateur embarqué, les chemins USB/charge, les protections contre retours de courant et le branchement de masse commun.
Séparer et découpler alimentation MCU et branches de moteurs (condensateurs près des pilotes et des charges), mesurer chutes/transitoires pour éviter resets BLE.

## Exigences retenues : sécurité et recharge du prototype porté

**Décisions de conception (2026-11-10)** — exigences à appliquer dès le banc de test ; leur réalisation reste à vérifier sur composants réels.

1. **Protection matérielle de la cellule LiPo 1S**, indépendante du firmware : surcharge, décharge excessive, surintensité et court-circuit. Employer une cellule protégée ou un ensemble cellule + carte de protection compatibles, avec limites documentées.
2. **Contrôleur de charge dédié et adapté à la cellule**, avec courant de charge limité et surveillance de température (thermistance/NTC si disponible et conforme à la fiche batterie). Ne pas confondre contrôleur de charge, circuit de protection et convertisseur 3,3 V.
3. **Protection physique** : batterie fixée dans un compartiment isolant, à l'écart des points de flexion et de compression du bracelet ; protéger câbles et connecteurs contre abrasion, écrasement et traction. Éviter toute pièce saillante ou perforante proche de la cellule.
4. **Surveillance logicielle complémentaire** : tension batterie, température et éventuellement courant/état de charge, alertes et arrêt contrôlé. Ce logiciel n'est **jamais** l'unique barrière de sécurité.
5. **Batterie amovible pour V0.1 et recharge hors du gant**, avec connecteur polarisé protégé contre inversion/court-circuit et débranchement hors tension. Ne pas porter le prototype pendant la recharge.
6. **Première mise au point avec alimentation de laboratoire limitée en courant**, avant toute intégration LiPo sur le bras.
7. **Aucun fonctionnement sur le corps avant contrôles** des protections, du courant de pointe, de la température, des connexions, de l'absence d'échauffement et des modes de défaillance.

Une cellule LiPo garde un risque résiduel même protégée ; les tests et la sélection d'un boîtier adapté sont indispensables.

## Contraintes de tension adoptées
- **Objectif : rail régulé 3,3 V commun à l'électronique** (ESP32-C3, logique I²C, capteurs, pilotes DRV2605L), avec masse commune et découplages appropriés.
- **Batterie LiPo 1S non régulée** : tension variable, jamais à raccorder directement à la broche 3V3.
- **Chargeur séparé** : entrée USB 5 V utilisée uniquement pour la charge externe ; pas de recharge sur le gant V0.1.
- **Actionneurs** : tension effective et surcommande gérées par le pilote selon les limites de chaque moteur (en particulier 3,3 V maximal pour PMD 310-118.001).
- Ce choix n'interdit pas les tensions nécessaires au chargeur ou au circuit interne du convertisseur : « tout en 3,3 V » qualifie le **rail utilisateur**, pas la tension brute de batterie.

## Questions à résoudre
1. Choix d'un module buck-boost 3,3 V réellement disponible, rendement utile à faible charge, courant de pointe et protections.
2. Référence LiPo protégée, courant de décharge et module de charge/power-path.
3. Vérification du DRV2605L sur moteur 3 V (calibration + freinage, limites d'overdrive).
4. Mesure du courant moyen et maximal sur prototype 2 canaux.
