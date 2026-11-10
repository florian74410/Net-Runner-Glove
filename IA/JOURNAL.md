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
