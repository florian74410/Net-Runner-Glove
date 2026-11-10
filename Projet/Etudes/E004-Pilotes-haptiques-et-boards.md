# E004 — Pilotes ERM/LRA et cartes de développement

**10 novembre 2026.** Étude documentaire des contrôleurs, basée sur spécifications publiées. Aucun matériel testé ni référence commandée. Objectif NRP : deux moteurs ERM interchangeables, rail régulé 3,3 V, freinage actif, I²C, faible encombrement et économie d'énergie.

## Circuits intégrés (puce nue, hors moteur)

| Circuit | VDD | Courant veille typique | Courant réveillé sans signal typique | Architecture | Boîtier / intérêt | Référence |
|---|---|---|---|---|---|---|
| TI DRV2605L | 2–5,2 V | 4,1 µA en standby / 4 µA shutdown | 0,5 mA | 1 canal ERM ou LRA ; effets ROM, I²C, freinage actif | VSSOP 3 × 3 mm corps / DSBGA 1,5 × 1,5 mm ; nombreuses bibliothèques | https://www.ti.com/product/DRV2605L |
| TI DRV2604L | 2–5,2 V | À confirmer dans datasheet | À confirmer | 1 canal ERM/LRA ; mémoire RAM personnalisable, I²C, freinage | VSSOP / DSBGA ; mieux pour grammaires personnalisées à étudier | https://www.ti.com/product/DRV2604L |
| TI DRV2625 | 2,7–5,5 V | 1,55 µA standby / 0,105 µA shutdown | 2,5 mA | 1 canal ERM/LRA ; ROM effets, I²C, veille auto, freinage | DSBGA ~1,96 mm² ; carte EVM 10×10 mm | https://www.ti.com/product/DRV2625 |
| TI DRV2624 | 2,7–5,5 V | Shutdown 0,105 µA | À confirmer | 1 canal ERM/LRA, mémoire RAM pour effets | DSBGA ; alternative orientée bas niveau personnalisé | https://www.ti.com/product/DRV2624 |
| Renesas DA7280 | 2,8–5,5 V | 0,36 µA en idle-power avec rétention mémoire | À confirmer | 1 canal ERM/LRA, effets personnalisés, I²C | QFN-12 3×3 mm ou WLCSP ; kit constructeur relativement complexe | https://www.renesas.com/en/products/da7280 |

**Ne pas mélanger les états de consommation** : courant de shutdown, standby avec mémoire conservée et actif sans vibration sont distincts ; les mesures des fabricants diffèrent. Consommation moteur non incluse. Pour le NRP mesurer énergie *veille → démarrage → effet → retour veille*.

## Cartes de développement identifiées

| Carte | Dimensions indiquées | Alimentation | Prix public vu (indicatif) | Commentaire et source |
|---|---|---|---|---|
| **TI DRV2625EVM-MINI** | **10×10 mm** | 2,7–5,5 V (chip) | **31,90 USD** | Très compacte, un pilote / moteur ; https://www.ti.com/tool/DRV2625EVM-MINI |
| **Adafruit #2305 DRV2605L** | **25,8×17,8×4,6 mm** (version STEMMA QT) | 3,3 V / 5 V | **8,38 € TTC** chez DigiKey France | Facilité d'intégration, code Arduino ; https://www.adafruit.com/product/2305 ; https://www.digikey.fr/fr/products/detail/adafruit-industries-llc/2305/5356831 |
| **SparkFun ROB-14538** | Dimensions PCB non vérifiées ; ne pas utiliser dimensions de colis distributeur | 2–5,2 V (chip) | **12,73 € TTC** DigiKey France | Fichiers schéma/Eagle et bibliothèque Arduino ; https://www.sparkfun.com/sparkfun-haptic-motor-driver-drv2605l.html |
| **Pimoroni PIM452** | 23,5×19×8 mm avec connecteurs | 3,3 V / 5 V | 13,95 € | Inclut moteur **LRA** ; moins adapté au banc de moteurs ERM interchangeables ; https://shop.pimoroni.com/en-eu/products/drv2605l-linear-actuator-haptic-breakout |
| Seeed Grove Haptic Motor | Non vérifié | 3,3–5 V | Non confirmé | DRV2605L, 123 effets, disponible selon vendeur ; https://wiki.seeedstudio.com/Grove-Haptic_Motor/ |
| Renesas DA728x-EVAL-KIT | Non vérifié, carte mère USB et cartes filles | USB 5 V pour le kit | Non confirmé | Développement/diagnostic, moins compact ; https://www.renesas.com/en/design-resources/boards-kits/da728x-eval-kit |

**Prix et disponibilité non garantis** ; port et taxation peuvent renverser le classement coût global.

## DRV2605L : données électriques complémentaires
TI datasheet https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- 1 actionneur par pilote ; freinage et overdrive automatiques disponibles après configuration correcte.
- Standby typique 4,1 µA ; shutdown 4 µA ; veille éveillée sans signal 0,5 mA typique.
- 3,3 V d'alimentation est compatible ; régler tension nominale, OD_CLAMP et calibration selon moteur ERM choisi. Ne pas laisser au pilote la liberté de dépasser le **maximum du moteur**.
- La consommation en vibration dépend de l'ERM et de la forme d'onde, non des seuls µA de veille.
- Adresse I²C 0x5A non configurable : pour deux pilotes identiques, employer multiplexeur TCA9548A ou bus physiques distincts si carte MCU le permet. La commutation des commandes **ne multiplexe pas l'alimentation moteur**.

## Carte de multiplexage I²C
Adafruit TCA9548A #2717 : **30,6 × 17,6 × 2,7 mm**, prix **6,95 USD**, utilisable à 3,3 V : https://www.adafruit.com/product/2717

Pour seulement 2 canaux, comparer encombrement réel et coût à deux bus I²C ESP32 distincts ou à un autre pilote avec interface appropriée. Envisager TCA9548A lorsqu'on veut grandir vers 5 canaux.

## Conclusion d'étude
- **Meilleur point de départ facile : 2× Adafruit DRV2605L #2305** (et solution pour gérer adresse I²C identique).
- **Option miniaturisée mais chère à l'unité : TI DRV2625EVM-MINI (10×10 mm)**, sans avantage prouvé en autonomie moyenne avant test.
- **Alternative à investiguer pour NRP-Language : DRV2604L et DRV2624**, RAM d'effets personnalisés.
- Ne pas modifier une BOM en prix définitifs avant sélection des moteurs, carte MCU et solution I²C.

## Prochain protocole
Évaluer 3 paires d'ERM sur deux canaux ; vérifier le freinage et l'overdrive, consommation moyenne moteur + pilote, différences perceptibles, compatibilité avec séquences personnalisées et courants transitoires ; fonctionnement d'abord sur alimentation de laboratoire limitée en courant.
