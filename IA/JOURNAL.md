# Journal des travaux

## 2026-11-10 — Initialisation de la mémoire de projet
- Création des deux espaces de documentation `Projet/` et `IA/`.
- Formalisation du périmètre V0.1, de la BOM ECO indicative et des hypothèses de consommation.
- Enregistrement des choix et des points ouverts.
- Aucun composant commandé ni prototype physiquement testé dans cette étape.

### Prochaine reprise
Relire l'état, vérifier les changements Git, puis étudier les petits pilotes haptiques, la qualité des moteurs/capteurs et la mesure énergétique avant d'arrêter une nomenclature de commande.

## 2026-11-10 — Classement des dossiers de projet
- Ajout de `Projet/BOM/`, `Projet/Etudes/`, `Projet/Developpement/`, `Projet/Documentations/`.
- Déplacement de `Projet/BOM-V0.1.md` vers `Projet/BOM/BOM-V0.1.md`, contenu conservé.
- Création d'un inventaire d'antériorités avec liens et statut exploratoire, pour enrichissement au fil des recherches.
- Conservation des autres documents à la racine de `Projet/`.

## 2026-11-10 — Étude E001 sur les actionneurs haptiques
- Analyse documentaire des gants bHaptics, SenseGlove, MANUS, HaptX et revue systématique.
- Étude de comparaison ERM/LRA (Georgia Tech) et examen de références LRA miniatures.
- Dossier : `Projet/Etudes/E001-Actionneurs-haptiques.md` ; aucune référence encore commandée/validée physiquement.

## 2026-11-10 — Étude E002 ERM miniatures
- Orientation provisoire : ERM coin Ø8–10 mm pour le gant ; présélection de six références fabricant.
- Distinction vitesse d'arrêt libre/freinage actif, MOSFET vs pilote DRV2605L.
- Étude archivée : `Projet/Etudes/E002-Selection-ERM-miniatures.md` ; aucun achat décidé.

## 2026-11-10 — Datasheets et banc deux canaux
- Ajout `Projet/Documentations/Datasheets/README.md` avec liens et statut de téléchargement des 3 références ERM.
- Alerte document : la fiche PDF associée au NFP-FLAT-C0816 est en réalité étiquetée **NFP-C0815**, à ne pas utiliser comme fiche exacte.
- Option banc de test : 3 paires de moteurs (2 pièces de chaque modèle) interchangeables sur **2 canaux DRV2605L** ; 4 états binaires dont 3 actifs ; aucun achat validé.

## 2026-11-10 — Contrôle des datasheets reçues
- Deux PDF utilisateur vérifiés localement : PMD 310-118.001 (6 p., R002-V009) et NFP-FLAT-C1030 (5 p.). Empreintes SHA-256 et caractéristiques extraites dans `Projet/Documentations/Datasheets/README.md`.
- Le dessin C0815 transmis montre 1,6 ±0,1 mm d'épaisseur ; ne pas assimiler automatiquement C0815 à C0816.
- Transfert PDF binaire dans le dépôt encore à effectuer ; liens et inventaire enregistrés sans prétendre que les PDF sont présents.

## 2026-11-10 — PDF effectivement déposés dans GitHub
- Vérification du dossier `Projet/Documentations/Datasheets/` sur `main` : `datasheet_ERM-310-118.pdf` (1 449 686 octets) et `Datasheet_NFP-FLAT-C1030-20211116.pdf` (143 926 octets).
- Registre `Datasheets/README.md` mis à jour vers les liens réels ; la fiche exacte du C0816 reste à obtenir.

## 2026-11-10 — Hypothèse bus unique 3,3 V
- Proposition d'alimenter MCU, pilotes, capteurs et multiplexeur par un **rail régulé 3,3 V** via buck-boost depuis LiPo 1S ; sortie USB de charge 5 V distincte.
- Vérifier tension d'overdrive moteur, limites de courant, condensateurs de découplage et carte ESP32 précise.
- Étude archivée : `Projet/Etudes/E003-Alimentation-3V3.md` ; pas de montage validé.

## 2026-11-10 — Décision sécurité LiPo et alimentation commune
- Validation du principe **rail régulé 3,3 V commun** ; une LiPo 1S ne fournit pas directement une tension stable de 3,3 V.
- Quatre protections adoptées : cellule protégée matériellement, contrôleur de charge adapté, protection mécanique du boîtier, surveillance logicielle complémentaire.
- **Batterie amovible et recharge hors du gant** retenues pour le prototype V0.1 ; pas de port pendant recharge.
- Tests préliminaires sur alimentation de laboratoire limitée en courant, puis vérifications avant port.
- Documents actualisés : `Projet/Architecture-V0.1.md`, `Projet/Etudes/E003-Alimentation-3V3.md`, `Projet/README.md` et `IA/DECISIONS.md`.

## 2026-11-10 — Étude E004 pilotes haptiques
- Étude des drivers TI DRV2605L/2604L/2625/2624 et Renesas DA7280 ; comparatif de cartes Adafruit, TI, SparkFun, Pimoroni et Seeed.
- Contrôle des consommations selon états veille/actif, mini-board TI DRV2625EVM-MINI 10×10 mm.
- Étude : `Projet/Etudes/E004-Pilotes-haptiques-et-boards.md` ; sélection finale et achats non décidés.

## 2026-11-10 — Étude E005 cartes compactes DRV2605L / PCB
- Orientation : DRV2605L par ERM + multiplexeur TCA9548A + dictionnaires haptique en mémoire MCU.
- Comparaison mini-board TI 16,33 × 8,97 mm, Adafruit, SparkFun, Boardoza et Pimoroni PIM452 (LRA intégré, non idéal pour ERM interchangeable).
- Schémas constructeur et fichiers Eagle open hardware référencés dans `Projet/Etudes/E005-Cartes-DRV2605L-et-PCB.md`.
- PCB maison envisagé ; pas de schéma final ni achat décidé.

## 2026-11-10 — Archivage de référence DRV2605L
- Création du dossier `Projet/Documentations/Datasheets/DRV2605LDGSEVM-M/` et de son registre (SLOU396, SLOU502B, déclaration UE RoHS). Documents TI fournis et examinés ; **PDF non redistribués dans Git public** en raison des conditions de reproduction.
- Copie **intégrale** des huit fichiers du dépôt matériel `adafruit/Adafruit-DRV2605-PCB` (branche master) dans `Projet/Documentations/Datasheets/Adafruit-DRV2605-PCB/`, licence et attribution conservées.
- Le lien E2E `https://e2e.ti.com/f/1/t/1605518` est inaccessible à l'analyse ; son contenu reste à examiner.
