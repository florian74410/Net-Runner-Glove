# E006 — DRV2605L : signalement de démarrage et PCB NRP 2→5 canaux

**10 novembre 2026 — Revue de risque, pas un défaut de série confirmé.**

## Source utilisateur : discussion TI E2E
https://e2e.ti.com/f/1/t/1605518

Le texte communiqué par l'utilisateur indique deux symptômes dans des appareils fabriqués depuis plusieurs années : (1) certaines puces DRV2605L ne démarrent qu'après chauffage de quelques dizaines de degrés, puis restent opérationnelles tant que l'alimentation demeure ; remplacer la puce répare ce cas ; (2) après une coupure de 3 secondes, certains circuits restent lisibles sur I²C mais refusent les écritures de registres comme 0x01 ; après plus de 10 secondes hors tension, fonctionnement normal.

**Statut : témoignage d'un auteur, sans réponse TI confirmée ni erratum identifié pour ces cas.** Ne pas conclure à un problème sur tous les lots. Causes possibles à explorer : pièce endommagée/défectueuse, séquence de mise sous tension, broche EN, alimentation résiduelle / retour d'alimentation via lignes I²C, composant voisin sensible à la température ou autre défaut d'assemblage. L'échauffement de la puce ne constitue pas une solution à adopter.

## Faits documentés par TI
- DRV2605L datasheet : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- Guide de démarrage : https://www.ti.com/lit/an/sloa189/sloa189.pdf
- Stabiliser l'alimentation et attendre **au moins 250 µs** avant commandes I²C.
- Maintenir **EN à l'état haut** pour permettre l'écriture de registres ; acquittement I²C n'implique pas toujours prise en compte des données si EN est bas.
- Désactiver STANDBY (bit 6, registre 0x01), vérifier que les écritures sont réellement retenues par lecture de retour.
- Reset logiciel DEV_RESET dans le registre 0x01 ; si le bus est bloqué, prévoir une coupure/réinitialisation réelle de l'alimentation pilote et tester les durées de décharge. Prévoir un circuit qui évite le rétro-alimentation via SDA/SCL.
- Un bus 3,3 V ne permet **pas** la programmation OTP interne (TI demande 4–4,4 V pour OTP). Stocker les résultats de calibration dans l'ESP32 et les réappliquer au démarrage, sans OTP.

## Protocole de robustesse proposé
1. Mesurer VDD, EN, niveaux I²C et état des rails lors d'allumages à froid.
2. Répéter interruptions courtes (1 s / 3 s / 10 s) et plus longues, puis lecture de l'identifiant et test lecture/écriture de registre.
3. Couper et rétablir l'alimentation du pilote sans retour par les E/S.
4. Journaliser taux d'échec initialisation, version et provenance du lot de puces, température ambiante et courant de démarrage.
5. Tester protections d'alimentation et absence de moteur actif après échec ; essais sur banc avant port, sans provoquer de chauffe artificielle.

## Architecture PCB envisagée
- PCB central 2 couches regroupant **5 empreintes DRV2605L**, chacune pouvant être peuplée ou laissée vide ; **2 pilotes équipés en prototype V0.1**.
- **TCA9548A** pour bus I²C extensible de 2 à 5 moteurs sans réécriture du contrôleur d'accès aux canaux.
- Pas de MCU soudé sur le PCB : connexion à la carte ESP32 externe.
- Rail régulé **3,3 V** ; puissance moteur limitée selon datasheet et calibration.
- Connecteurs détachables pour moteurs, condensateurs locaux, résistances de rappel I²C à dimensionner et points de test 3V3/GND/EN/SDA/SCL.
- Étudier gestion indépendante EN et éventuellement coupure électrique des pilotes, pour diagnostic et redémarrage. La coupure des pilotes doit s'assurer qu'aucun chemin SDA/SCL ne les réalimente.
- Batterie LiPo protégée amovible, chargée **hors du gant**, non directement connectée au rail 3,3 V.

## Fabrication JLCPCB
JLCPCB annonce **à partir de 2 USD pour 5 PCB nus** standard à deux couches de taille <=100×100 mm (prix promotionnel avant frais et options) : https://jlcpcb.com/help/article/how-do-i-place-an-order
La conception, le PCB assemblé, les composants, la soudure, taxes et expédition **ne sont pas inclus**. Vérifier compatibilité règles de routage, boîtier DRV2605L VSSOP, disponibilité TCA9548A, minimum de commande et coût final dans le devis Gerber réel.

## Options conservées pour BOM V0.2
A. **2× cartes Adafruit DRV2605L + multiplexeur** : développement rapide, plus volumineux.
B. **PCB NRP 5 emplacements, 2 puces montées + multiplexeur** : plus compact, préparation et tests nécessaires ; cinq PCB nus bon marché possibles selon options.

**Décision :** étudier les deux sans les présenter comme validées. Ajouter conditions de démarrage fiable et tests de coupure d'alimentation au cahier des charges.
