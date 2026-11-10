# ESP32-C6-MINI-1 — Fiche technique française pour NRP-HC-01

**Source primaire :** *ESP32-C6-MINI-1 / ESP32-C6-MINI-1U Datasheet*, Espressif Systems, **v1.5**, datée du **16 juin 2026**, **53 pages PDF** (52 pages numérotées + couverture). Document transmis par l'utilisateur le 10 novembre 2026.

**Document constructeur original :** https://espressif.com/documentation/esp32-c6-mini-1_mini-1u_datasheet_en.pdf

**Statut du PDF dans ce dépôt :** lien officiel et fiche d'analyse disponibles. Le PDF original fourni dans la conversation n'est **pas encore copié dans Git** : l'interface Git disponible ne permet pas de transmettre directement un fichier binaire local. Ne pas déclarer le PDF archivé avant de le vérifier dans le dépôt.

**Périmètre :** données strictement tirées du document Espressif, avec renvoi vers les **pages PDF** entre parenthèses. Les propositions NRP sont explicitement distinguées.

## 1. Résumé des caractéristiques

| Paramètre | Donnée constructeur | Pages PDF |
|---|---|---|
| Famille | ESP32-C6-MINI-1 avec antenne PCB ; MINI-1U avec connecteur RF externe | 1, 3 |
| Processeur | ESP32-C6FH4/FH8 ; **RISC-V 32 bits monocœur**, **jusqu'à 160 MHz** | 2, 4 |
| ROM | **320 Ko** | 2 |
| SRAM principale haute performance | **512 Ko** | 2 |
| SRAM basse consommation | **16 Ko** | 2 |
| Flash intégrée au boîtier de puce | **4 Mo** (N4/H4) ou **8 Mo** (H8), Quad SPI | 4 |
| GPIO | **Jusqu'à 22** broches ; disponibilité réelle dépend de l'affectation | 1, 10–11 |
| ADC | **SAR 12 bits**, entrée possible sur **7 GPIO** (IO0 à IO6) | 11, 24 |
| I²C | **2 contrôleurs** (système principal + basse consommation), standard **100 kbit/s**, fast **400 kbit/s** | 19 |
| USB | USB Serial/JTAG **Full Speed 12 Mbit/s**, CDC-ACM ; périphérique USB natif | 20 |
| BLE | **Bluetooth LE 5.3 certifié**, Bluetooth mesh, PHY **125/500 kbit/s, 1/2 Mbit/s** | 2 |
| Radio BLE | **2402–2480 MHz**, puissance TX **–16 à +19 dBm** | 33 |
| Wi-Fi | 2,4 GHz 802.11b/g/n/ax (Wi-Fi 6), utilisable mais non requis pour NRP | 1–2 |
| IEEE 802.15.4 | Zigbee 3.0 et Thread 1.3 pris en charge | 3 |
| Alimentation module | **3,0 à 3,6 V**, nominal **3,3 V** | 3, 26 |
| Courant que l'alimentation externe doit pouvoir fournir | **au moins 0,5 A** pour le **module**, selon tableau des conditions recommandées ; **ne pas confondre avec courant moyen** | 26 |
| Dimension MINI-1 | **13,2 × 16,6 × 2,4 mm** | 4, 41 |
| Dimension MINI-1U | **13,2 × 12,5 × 2,4 mm** | 4, 41 |
| Température N4 | **–40 à +85 °C** | 4 |
| Température H4 / H8 | **–40 à +105 °C** | 4 |
| Horloge intégrée au module | Quartz **40 MHz** | 3, 9 |
| Sensibilité ESD | HBM **±2 kV**, CDM **±500 V** | 46 |
| Sensibilité humidité | **MSL 3** ; souder dans les 168 h après ouverture dans conditions indiquées, sinon étuvage | 46 |

**Attention :** le document constructeur est celui du module ; il renvoie à la *ESP32-C6 Series Datasheet*, aux *Hardware Design Guidelines* et au *Technical Reference Manual* pour d'autres détails matériels, précisions ADC, certains brochages et recommandations RF.

## 2. Consommation — valeurs documentées

Mesures **3,3 V / 25 °C**. Les valeurs **TX/RX actives sont des pointes**, pas une moyenne d'utilisation NRP ; elles n'incluent ni les DRV2605L, ni les moteurs, ni l'alimentation et ses pertes.

| État radio / alimentation | Valeur constructeur | Page PDF |
|---|---:|---|
| BLE émission +19 dBm | **309 mA crête** | 27 |
| BLE émission +9 dBm | **189 mA crête** | 27 |
| BLE émission 0 dBm | **131 mA crête** | 27 |
| BLE émission –16 dBm | **94 mA crête** | 27 |
| BLE réception | **73 mA crête** | 27 |
| Modem-sleep, CPU actif 160 MHz, horloges périphériques coupées | **27 mA typiques** | 28 |
| Modem-sleep, CPU idle 160 MHz, horloges coupées | **17 mA typiques** | 28 |
| Modem-sleep, CPU actif 80 MHz, horloges coupées | **19 mA typiques** | 28 |
| Modem-sleep, CPU idle 80 MHz, horloges coupées | **14 mA typiques** | 28 |
| Light-sleep, cas du tableau | **180 ou 35 µA typiques** selon état listé | 28 |
| Deep-sleep | **7 µA typiques** | 28 |
| Chip disabled (EN bas) | **1 µA typique** | 28 |

**Conception :** dimensionner l'alimentation pour les pointes BLE et les démarrages moteurs simultanés ; l'autonomie moyenne doit être **mesurée** à fréquence d'échantillonnage et profil de trafic BLE réellement utilisés. La description des sous-cas Light-sleep est à relire avec la table 6-8 du PDF, plutôt que d'interpoler une valeur unique.

## 3. Brochage — fonctions NRP et pièges

D'après le tableau 3-1 (PDF p. 10–11) :

| Broche module | Nom | Fonctions / précautions |
|---:|---|---|
| 3 | 3V3 | Alimentation stabilisée de **3,0–3,6 V** |
| 8 | EN | HIGH = activé ; LOW = éteint / reset ; **ne pas laisser flottant** |
| 12 | IO0 | **ADC1_CH0** |
| 13 | IO1 | **ADC1_CH1** |
| 5 | IO2 | **ADC1_CH2** |
| 6 | IO3 | **ADC1_CH3** |
| 9 | IO4 | **ADC1_CH4**, également MTMS |
| 10 | IO5 | **ADC1_CH5**, également MTDI |
| 15 | IO6 | **ADC1_CH6**, également MTCK / LP_I2C_SDA |
| 16 | IO7 | MTDO / LP_I2C_SCL |
| 17 | IO12 | **USB D−** |
| 18 | IO13 | **USB D+** |
| 22 | IO8 | **Strapping** (boot / logs) |
| 23 | IO9 | **Strapping** (boot/download), pull-up interne faible par défaut |
| 20 | IO15 | Strapping JTAG |
| 30 | RXD0 | GPIO17, UART0 RX |
| 31 | TXD0 | GPIO16, UART0 TX |
| 1, 2, 11, 14, 36–53 | GND | Broches de masse ; pad central selon recommandation |
| 4, 7, 21, 32–35 | NC | Non connectées ; ne pas affecter |

**Important :** le nombre *22 GPIO* est une capacité du module, pas 22 broches libres une fois USB, boot, UART, ADC et extensions réservés.

**Strapping (PDF p. 12–16) :** GPIO8/GPIO9 commandent le boot (GPIO9=1 : SPI boot ; GPIO8=1 et GPIO9=0 : download boot) ; GPIO15 intervient au choix de source JTAG. Garder ces broches dans un état déterministe au reset. Attente minimale de maintien du strapping après reset **3 ms**. Rail stabilisé puis EN : délai minimal de stabilité indiqué **50 µs**, et impulsion reset EN bas au moins **50 µs**.

## 4. Schéma électrique de référence à reproduire dans KiCad

La **figure 9-1, page PDF 40**, fournit un exemple d'application et non une liste aveugle de composants requis :

- **3,3 V : condensateurs 22 µF et 100 nF** près du module dans l'exemple (C1/C2).
- **EN :** **Espressif** recommande généralement un **RC de 10 kΩ + 1 µF** pour sécuriser la séquence de démarrage ; valeurs à adapter si la montée du 3,3 V diffère.
- Connecter **GPIO12 (USB D−)** et **GPIO13 (USB D+)** à l'USB selon les recommandations de routage/ESD et la connectique choisie.
- Prévoir BOOT/RESET accessibles et une voie de récupération UART0 en plus du USB/JTAG.
- **Ne pas redessiner** la partie quartz RF et réseau d'adaptation de la *figure 8-1 page 38* : ils sont **déjà à l'intérieur du module ESP32-C6-MINI-1**.
- La figure 9-1 mentionne des éléments **NC**, non montés : ne pas copier toute valeur `TBD` comme si elle était validée.
- Le pad thermique/masse central peut être brasé pour optimiser la dissipation, selon recommandation d'application et quantité correcte de pâte (p. 40).

## 5. Antenne et PCB

- **MINI-1** : antenne PCB intégrée ; **MINI-1U** : connecteur pour antenne externe ; ce dernier n'est pas fourni avec l'antenne (p. 3, 42).
- **Empreinte mécanique officielle :** figure **11-1 page PDF 44**, pas de **0,8 mm** indiqué sur le dessin ; modèle STEP téléchargeable via ressources Espressif.
- **Positionnement :** la section 11.2 page PDF 45 renvoie expressément aux **ESP32-C6 Hardware Design Guidelines** pour la zone d'exclusion RF et la position sur PCB. **Aucune cote de dégagement RF de 15 mm ne doit être traitée comme une valeur tirée de cette datasheet** ; prendre la valeur exacte des guidelines lors du placement.
- Placer l'antenne sur un bord de carte et éviter batterie, cuivre et métal au voisinage selon les guides Espressif ; ces recommandations sont un **plan NRP à vérifier**, pas une cote extraite du PDF.
- Assemblage : **MSL 3**, profil de refusion illustré p. 46 ; éviter le nettoyage/soudage ultrasonique du module (p. 47).

## 6. Implications proposées pour HC-01 (décisions projet, pas caractéristiques constructeur)

1. **Variante candidate ESP32-C6-MINI-1-H8 :** antenne intégrée et flash 8 Mo ; référence définitive et stock à confirmer.
2. Rail **3,3 V régulé** conçu pour les **capacité d'au moins 0,5 A recommandée pour le module**, plus cinq DRV2605L et appels de courant ERM. Protection LiPo et recharge hors gant retenues dans le projet.
3. Affectation GPIO **provisoire à concevoir** ; réserver 5 ADC pour les FSR futurs, 2 fils I²C pour le TCA9548A, USB D± pour la récupération, états déterministes des strap pins, éventuellement commandes individuelles EN/load switches. Aucune affectation définitive avant validation.
4. Conserver points de test 3V3, GND, EN, UART RX/TX, USB si accessible, et I²C, dans des zones réparables.
5. PCB **cinq canaux haptique prévus, un seul DRV2605L peuplé initialement** ; bien séparer les symboles configurés en DNP/« ne pas monter » et les chemins alimentés.
6. Modulaire : PCB central/bracelet ESP32 + MUX + drivers + analogique, câble remplaçable vers gant essentiellement textile.

## 7. Contrôles avant fabrication

- [ ] Télécharger les **Hardware Design Guidelines ESP32-C6** et vérifier antenne, EN, USB, alimentation.
- [ ] Comparer empreinte KiCad à la **figure 11-1 p. 44** et modèle STEP officiel.
- [ ] Compiler tableau réel GPIO / ADC / boot, détecter collisions.
- [ ] Vérifier schéma 3,3 V, découplage local et **crêtes BLE + moteurs**.
- [ ] Faire revue ERC puis DRC ; mise à disposition de points de test et accès reprise CMS.
- [ ] Vérifier conditions d'assemblage MSL3.
- [ ] Construire un premier firmware BLE avec un canal I²C et mesure ADC avant le port du gant.

**Copyright et attribution :** cette fiche est un **résumé technique original en français**, et non une traduction intégrale ni une reproduction du PDF. Les schémas et figures constructeur sont à consulter dans le PDF original.
