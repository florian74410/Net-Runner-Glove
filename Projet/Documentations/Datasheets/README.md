# Datasheets — NRP

**10 novembre 2026** — Registre de documentation constructeur, séparant références vérifiées et documents en attente. Les PDF ne sont **pas encore hébergés ici** : fournir/upload les fichiers originaux avant archivage afin de garantir le bon modèle et de vérifier les droits de redistribution.

## Trois candidats ERM (2 échantillons chacun envisagés)

| Référence | PDF d'origine | Statut | Commentaire |
|---|---|---|---|
| Precision Microdrives **310-118.001**, Ø10 × 2,1 mm | https://precisionmicrodrives.com/cdn/catalog_product/a28a1e83-edad-45e1-bd6f-afa9c5fef1d4/310-118.pdf | **PDF identifié et modèle vérifié** | 6 pages, modèle 310-118.001 sur la page de titre ; temps de freinage documenté |
| NFP **FLAT-C0816**, Ø8 × 1,6 mm | https://nfpmotor.com/8mm-coin-vibration-motor-1-6mm-type-model-nfp-flat-c0816 | **PDF exact non trouvé** | Le bouton DATA SHEET des fiches vendeurs renvoie à un PDF étiqueté **NFP-C0815**, et **NON C0816**. Ne pas l'archiver sous un faux nom ou inférer ses caractéristiques exactes. Demander SPEC-NFP-FLAT-C0816 au fournisseur. |
| NFP **C1030**, Ø10 × 3 mm | https://temperosystems.com.au/wp-content/uploads/2023/01/SPEC-NFP-FLAT-C1030-20211116-watermark.pdf | **PDF trouvé, accès web bloqué (403)** | À télécharger par navigateur utilisateur, puis vérifier modèle et révision dans le fichier avant archivage. |

## Autres références utiles
- Pilote DRV2605L, Texas Instruments : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- Bibliothèque Arduino/ESP32 : https://github.com/adafruit/Adafruit_DRV2605_Library
- Documentation module Adafruit : https://cdn-learn.adafruit.com/downloads/pdf/adafruit-drv2605-haptic-controller-breakout.pdf

## Convention de dépôt (si permission de redistribution du document)
`Fabricant-Modele-Revision.pdf` ; noter lien de source, date de récupération, version et SHA-256 dans un inventaire. Éviter les PDFs mal étiquetés.

## Vérifications obligatoires
- Correspondance exacte modèle/variante, pas seulement diamètre ou caractéristiques voisines.
- Tension et courant de démarrage, durée de vie, fiche de tolérance.
- Conditions de mesure du G et temps de montée/descente.
- Compatibilité de l'actionneur avec le DRV2605L et limites de courant.
