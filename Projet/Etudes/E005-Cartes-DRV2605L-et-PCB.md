# E005 — Breakouts DRV2605L et architecture PCB NRP

**Date : 2026-11-10 — Étude documentaire, pas encore schéma NRP validé.**

## Décision d'orientation
- Driver : **TI DRV2605L**, un driver par ERM, avec freinage actif paramétré et test de calibration.
- Interface : **multiplexeur I²C TCA9548A** envisagé afin de conserver le même modèle d'accès (sélection du canal -> envoi I²C à 0x5A) en passant de 2 à 5 doigts.
- NRP-Language : motifs stockés dans la **mémoire ESP32**, avec émission de commandes I²C / mode RTP et effets internes si pertinents. Pas de besoin avéré d'un pilote distinct à RAM de formes d'onde.
- Rail : **3,3 V régulé**, moteurs commandés dans leurs limites nominales.
- Prototype modulaire : moteurs physiquement interchangeables, pas de moteur soudé au PCB de commande.
- **Avant la conception de PCB ou l'achat, vérifier brochages, licences des fichiers sources, disponibilité et besoins électriques réels.**

## Comparatif des cartes au DRV2605L

| Carte | Dimensions publiées | Actionneur | Schéma / conception disponibles | Évaluation NRP |
|---|---|---|---|---|
| **TI DRV2605LDGSEVM-M** | **16,33 × 8,97 mm** | externe | manuel TI avec schéma et BOM | **Référence la plus compacte identifiée** |
| **Adafruit DRV2605L #2305** | ~25,8 × 17,8 mm (selon révision) | externe | Eagle .sch/.brd open hardware | Accessible pour essai et adaptation |
| **SparkFun ROB-14538** | à vérifier sur la révision du PCB (ne pas inférer des dimensions du colis) | externe | PDF et sources Eagle v20 | Très bon exemple à étudier |
| **Boardoza DRV2605L** | 20 × 20 mm | externe | sources de conception publiques, CC BY-SA 4.0 matériel | Alternative open hardware |
| **Pimoroni PIM452** | **23,5 × 19 × 8 mm**, connecteurs inclus | **LRA ELV1411A intégré** | schéma à confirmer | Bon concept compact, mais **moins adapté** à nos trois paires ERM interchangeables |
| **Zio Qwiic Haptic** | 35,9 × 14,4 mm | LRA intégré | PCB/schéma annoncés | Pas prioritaire pour ERM |

## Liens primaires schémas et sources
- TI mini-board **DRV2605LDGSEVM-M** : https://www.ti.com/tool/DRV2605LDGSEVM-M
- TI guide miniature / BOM : rechercher `SLOU396 DRV2605LDGS Haptic Driver Mini Board` depuis la page TI.
- TI **datasheet DRV2605L** (notamment Figure 58 et Tableau 32, page imprimée 55) : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- Adafruit schéma + Eagle : https://github.com/adafruit/Adafruit-DRV2605-PCB
- Adafruit dessins et téléchargement : https://learn.adafruit.com/adafruit-drv2605-haptic-controller-breakout/downloads
- SparkFun schéma PDF : https://cdn.sparkfun.com/datasheets/Robotics/Haptic_Motor_Driver_DRV2605L.pdf
- SparkFun fichiers Eagle : https://github.com/sparkfun/Haptic_Motor_Driver
- Boardoza : https://github.com/Boardoza/Boardoza_DRV2605L_Haptic_Driver_Breakout_Board
- Pimoroni PIM452 : https://shop.pimoroni.com/en-eu/products/drv2605l-linear-actuator-haptic-breakout
- Bibliothèque Arduino Adafruit : https://github.com/adafruit/Adafruit_DRV2605_Library

## Schéma conceptuel d'un canal — **NON prêt à fabriquer**
```text
ESP32 I²C SDA/SCL
       |
       v
TCA9548A  (adresse configurable, branche 0..7)
       |-- CH0 SDA/SCL --> DRV2605L A (0x5A) --> ERM A
       |-- CH1 SDA/SCL --> DRV2605L B (0x5A) --> ERM B
       |-- CH2..CH4    --> futures voies
3V3 régulé -> VDD MCU + TCA9548A + VDD chaque DRV2605L
GND commun ; EN et IN/TRIG raccordés selon la configuration choisie
```

Un DRV2605L **ne pilote qu'un ERM indépendamment** ; le multiplexeur ne partage que la communication I²C. Les effets peuvent continuer après la désélection de la branche sous réserve du mode correctement paramétré. Éviter les conflits de pull-up sur les bus.

## Composants périphériques minimaux tirés du schéma constructeur
- DRV2605L : 1 par actionneur.
- **1 µF sur VDD**, près de la puce.
- **1 µF sur REG**, près de la puce (ne pas utiliser REG comme alimentation externe).
- Pull-up SDA/SCL (TI illustre 2,2 kΩ) selon topologie effective ; ne pas dupliquer sans calcul.
- Connexion EN ; IN/TRIG si nécessaire ; sortie différentielle OUT± vers moteur.
- Connecteurs moteurs débrochables et découplage du rail à dimensionner selon les courants transitoires.
- Gestion charge/protection LiPo et buck-boost 3,3 V **hors** de cette section pilote.

## Taille du futur PCB — objectif non validé
L'existence du TI mini-board de 16,33 × 8,97 mm prouve qu'un canal séparé peut être très petit. Un PCB central regroupant 2, puis 5, DRV2605L et le TCA9548A, ses passifs et connecteurs peut limiter la longueur des liaisons I²C tout en ne laissant que deux fils moteur par doigt. La place des connecteurs détachables, de la batterie et des passifs déterminera la taille réelle : **pas de dimensions annoncées pour NRP avant routage**.

## Firmware suggéré (concept)
- Une interface `HapticChannel` abstrait la sélection du canal.
- Sélection TCA9548A, configuration DRV2605L à 0x5A, émission d'un motif, désélection et sélection éventuelle d'un autre canal.
- Stockage sur le MCU des symboles NRP : doigt(s), durées, intensité et rythme ; bibliothèque ROM du DRV2605L utilisable sans l'imposer à NRP-Language.
- Prévoir verrou d'accès I²C, préemption des messages prioritaires, calibration par type moteur et synchronisation (le changement de canal successif crée une faible latence à mesurer).
- Les deux premiers canaux utilisent la même API que cinq canaux, par changement de configuration, non refonte du code.

## Questions ouvertes
- Référence précise de module TCA9548A ; pull-ups, adresse, dimensions et consommation.
- Pilote DRV2605L avec overdrive et freinage actif respectant 3,3 V maximum sur PMD 310-118.001.
- Si les bibliothèques d'effets sous licence Immersion peuvent être exploitées commercialement dans le cadre exact souhaité ; consulter les notices TI/licences avant commercialisation.
- Prix comparés : carte TI mini vs Adafruit vs PCB personnalisé (PCB nu, assemblage, minimum de commande, livraison).
