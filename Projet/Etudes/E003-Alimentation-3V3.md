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

## Questions à résoudre
1. Choix d'un module buck-boost 3,3 V réellement disponible, rendement utile à faible charge, courant de pointe et protections.
2. Référence LiPo protégée, courant de décharge et module de charge/power-path.
3. Vérification du DRV2605L sur moteur 3 V (calibration + freinage, limites d'overdrive).
4. Mesure du courant moyen et maximal sur prototype 2 canaux.
