# Datasheets — NRP

**Mise à jour : 2026-11-10 — 2 PDF reçus et vérifiés localement, en attente de transfert binaire dans GitHub.**

Ce registre consigne les fichiers transmis par l'utilisateur, leurs identités et empreintes ; **les deux PDF ne sont pas encore téléversés dans le dépôt**. Un lien vers un PDF hébergé chez le fabricant n'est pas une copie archivée dans Git.

## PDF fournis

| Fichier à archiver ici | Identité vérifiée | Pages | Taille | SHA-256 | Statut |
|---|---|---:|---:|---|---|
| `PrecisionMicrodrives-310-118.001-R002-V009.pdf` | PMD 310-118.001, R002-V009 ©2021 | 6 | 1 449 686 octets | `82ec623f5c8a006a6229c7c585f4fdb3431330fda976335c599ee827d8f7857e` | Vérifié localement ; binaire à pousser |
| `NFP-FLAT-C1030-20211116.pdf` | NFP-FLAT-C1030, Need-For-Power | 5 | 143 926 octets | `fffd43b26da4316f9b8593dfb7f8e145928d02d86ca8167f17ef1e28ca675341` | Vérifié localement ; binaire à pousser |

**Attention aux variantes :** `NFP-FLAT-C1030` (titre du document reçu) n'est pas nécessairement interchangeable avec toute référence commerciale `NFP-C1030` ; vérifier au vendeur.

## Quelques caractéristiques issues des PDF
- **310-118.001** (pages 3–4) : Ø10 ±0,1 mm ; épaisseur 2,1 ±0,1 mm ; 3,0 V nominal ; 3,3 V maximum ; 60 mA typique / 90 mA max. en régime ; 100 mA typique / 120 mA max. démarrage ; temps d'arrêt 128 ms, freinage actif 50 ms ; durée de montée 96 ms. *Ces mesures sont celles du fabricant sur sa charge étalon, pas sur la peau.*
- **NFP-FLAT-C1030** (pages 1–3) : 3,0 V nominal, plage 2,3–3,6 V, 80 mA max. régime, 120 mA max. démarrage, endurance testée sur 50 000 cycles (conditions fabricant). La feuille reçue ne fournit pas dans les parties textuelles de mesure d'accélération 1,4 G : **ne pas utiliser ce chiffre comme vérifié par ce PDF**.

## En attente
- **NFP-FLAT-C0816** : le fichier précédemment trouvé comme « datasheet C0816 » affiche **NFP-C0815**. Le dessin partagé du C0815 indique une épaisseur **1,6 ±0,1 mm**, et un diamètre 8,0 ±0,1 mm ; cela ne valide pas automatiquement les caractéristiques du C0816. Demander une datasheet libellée exactement `NFP-FLAT-C0816`.

## Sources
- PMD : https://precisionmicrodrives.com/cdn/catalog_product/a28a1e83-edad-45e1-bd6f-afa9c5fef1d4/310-118.pdf
- NFP C1030 : https://temperosystems.com.au/wp-content/uploads/2023/01/SPEC-NFP-FLAT-C1030-20211116-watermark.pdf
- Pilote DRV2605L : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- Bibliothèque Arduino Adafruit : https://github.com/adafruit/Adafruit_DRV2605_Library

## Archivage
Préserver l'original binaire ; enregistrer l'empreinte SHA-256 ; vérifier l'autorisation de redistribution avant publication sur ce dépôt public. Ne pas renommer un PDF comme s'il concernait une autre variante moteur.
