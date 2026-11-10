# E007 — Capteurs de pincement pouce-doigts : interface Homme → Machine

**2026-11-10. Étude documentaire ; pas de prototype testé.**

## Fonctions à distinguer
1. **Contact / identification** : savoir quel(s) doigt(s) est/sont touché(s) par le pouce.
2. **Pression relative** : estimer l'intensité du pincement, pour des seuils doux/fort et éventuellement une valeur analogique.
3. **Qualité du geste** : temps, relâchement, anti-rebond, contacts multiples, artefacts de pliage, fatigue.

## Candidats
| Technologie | Exemple | Dimensions et données établies | Avantages | Limites |
|---|---|---|---|---|
| FSR petit diamètre | Interlink **FSR 400** | Zone sensible Ø**5,1 mm**, épaisseur **~0,35 mm** (datasheet historique), actuation annoncée **~0,1 N**, gamme jusqu'à **20 N** selon fabricant/version | Très compact, sortie résistive 2 fils, pression graduelle | Non linéaire, dérive, pression de contact et placement critiques, étalonnage individuel |
| FSR film précis | **Tekscan FlexiForce A201** | Épaisseur **0,203 mm**, zone Ø**9,53 mm**, largeur **14 mm**, longueur standard **191 mm** (versions plus courtes disponibles) ; variantes **4,4 N / 111 N / 445 N** | Souple, gamme calibrable, mesure d'effort documentée | Bande encombrante, prix supérieur ; attention pression ponctuelle |
| Film résistif DIY | **Velostat/Linqstat** | Feuille **0,1 mm** ; découpe libre ; deux électrodes et couche sensible | Très économique, peut être cousu et dimensionné librement | Forte variabilité, dérive, déformations, isolation/assemblage et stabilité à éprouver |
| Contacts conducteurs | Pastilles textiles, fil conducteur | 1 commun pouce + 4 entrées doigts ; pas de mesure d'effort | Ultra simple, consommation statique faible, gestes discrets | Contact accidentel, oxydation/humidité, anti-rebond ; pas de force |
| Capacitif textile | Électrodes + contrôleur capacitif, à qualifier | Paramètres non validés dans cette étude | Détection sans pression et sans contact galvanique | Proximité ambiguë, eau et corps, calibration ; la force n'est pas directement mesurée |

### Sources primaires
- Interlink FSR400 : https://www.interlinkelectronics.com/fsr-400
- Interlink FSR overview : https://www.interlinkelectronics.com/force-sensing-resistor
- Tekscan FlexiForce A201 : https://www.tekscan.com/products-solutions/force-sensors/flexiforce-a201-sensor-0
- Tekscan datasheet : https://cdn.sparkfun.com/datasheets/Sensors/ForceFlex/FLX-A201-A.pdf
- Adafruit guide FSR (limites de précision) : https://learn.adafruit.com/force-sensitive-resistor-fsr
- Adafruit Velostat : https://www.adafruit.com/product/1361
- Revue des gants intelligents, points tactiles pouce/doigts : https://doi.org/10.3390/s21082667
- Fraunhofer : gant avec capteurs capacitifs textiles : https://www.cesma.de/en/applications/e-textiles/sensor-glove.html

## Recommandation de protocole progressif
**Étape A : contact binaire**, quatre zones conductrices sur index, majeur, annulaire, auriculaire et zone commune sur pouce. Détecter l'état des contacts, y compris plusieurs contacts à la fois, tester les faux positifs, la fatigue et la robustesse du textile.

**Étape B : pression**, ajouter initialement **2 FSR400** (ou un FSR400 + une pastille Velostat de comparaison) pour mesurer niveau de pincement léger/fort. Le FSR sera protégé et placé sur une zone d'appui mécanique reproductible ; éviter des couches qui empêchent la fermeture des électrodes de contact. Étalonner sur le gant porté, pas uniquement sur table.

**Étape C : 4 voies**, ajouter jusqu'à quatre FSR si l'essai démontre un gain. Les plages et seuils par doigt sont configurables dans le MCU ; les commandes NRP-Language doivent rester abstraites de la technologie de capteur.

### Électronique de lecture et alimentation
- Chaque FSR est une résistance variable. Un pont diviseur 3,3 V + résistance fixe fournit une tension mesurable par ADC ; le pont consomme lorsqu'il est alimenté : possibilité d'excitation seulement au moment des échantillons, avec temps de stabilisation à caractériser.
- **ESP32-C3** : ADC1 sur **GPIO0 à GPIO4**, cinq canaux de mesure ; broches réellement disponibles à confirmer pour la carte exacte et ses fonctions annexes. Référence https://documentation.espressif.com/esp32-c3_datasheet_en.html
- Alternative extensible : **ADS1115**, 4 entrées analogiques I²C (pas 5), 16 bits, 860 SPS max total, ~150 µA en conversion continue ; deux adresses possibles sur le même bus, ou deux circuits si >4 analogiques. Référence https://www.ti.com/product/ADS1115
- **Attention :** l'ADS1115 partage le bus **amont** du TCA9548A ou une branche dédiée. Les capteurs analogiques ne se connectent pas au bus I²C ; ne pas confondre MUX numérique I²C et multiplexage analogique.
- Contacts textiles via GPIO + résistances adaptées, entrées protégées ESD ; attention au schéma et à la détection fiable de plusieurs doigts simultanés.
- **Câblage gant-bracelet** : selon architecture, au moins un conducteur par électrode + commun de contact, deux conducteurs par FSR si lecture centralisée, 2 fils par ERM ; le nombre réel de voies conditionnera le connecteur. Ne pas présumer qu'un câble ultra-fin supportera des dizaines de cycles de pliage.

## Modèle logiciel suggéré
`NRPInputEvent {finger, contact, force_normalized, duration_ms, confidence}` ; mapping des événements vers NRP-Language par profil. Seuils dépendant de chaque doigt, anti-rebond, hystérésis et mode sûr à défaut de capteur.

## Critères pratiques à tester
- Seuil minimal fiable, faible pincement vs fort pincement, réaction aux gestes rapides et répétés.
- Faux positifs lors de fermeture naturelle de la main, marche, transpiration, frottements du textile.
- Usage ambidextre, confort du pouce, douleur ou pression prolongée ; aucune obligation de forcer le geste.
- Nettoyage du gant, réparation de fils et remplacement individuel des capteurs.
- Coût du capteur **et** du connecteur, montage, boîtier, longueur de queue et câble.
- Essais non portés initialement pour vérifier courant et protections électriques.

## Décision du 10 novembre 2026
**Pas de modèle acheté ni arrêté** : shortlist initiale FSR400 et Velostat, avec contacts textiles pour identification, FlexiForce A201 comme référence plus précise mais coûteuse. Priorité à la modularité, à la réparabilité et à la capacité de modifier le gant sans changer le protocole NRP.
