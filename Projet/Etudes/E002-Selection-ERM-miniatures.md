# E002 — ERM coin pour les doigts : présélection

**10 novembre 2026 — Étude exploratoire, aucun achat validé.**

## Cahier des charges
- Diamètre maximal : 10 mm ; épaisseur minimale souhaitée.
- Effet tactile suffisamment fort mais confortable ; comparaison à intensité *perçue*, pas seulement en G sur banc.
- Privilégier composants avec données publiées de courant, temps de montée et freinage.
- Actionneur seul sur le doigt ; électronique/pilote sur bracelet ; liaisons démontables et flexibles.
- ERM retenu comme **orientation**, LRA encore disponible comme référence de comparaison.

## Candidats (données fabricants)

| Modèle | Dimensions | Amplitude annoncée | Courant annoncé | Tension | Source |
|---|---|---|---|---|---|
| NFP-FLAT-C0816 | Ø8 × 1,6 mm | ≥0,5 G | ≤80 mA | 3 V | https://precisionminidrives.com/product/8mm-coin-vibration-motor-1-6mm-type-model-nfp-flat-c0816 |
| NFP-C0820(L) | Ø8 × 2,2 mm | 0,55 G | ≤80 mA | 3 V | https://precisionminidrives.com/product/8mm-coin-vibration-motor-2mm-type-model-nfp-c0820 |
| NFP-C0827(L) | Ø8 × 2,7 mm | 0,75 G | 69 mA typ.; 100 mA max. | 3 V | https://precisionminidrives.com/product/8mm-button-coin-vibration-motor-2-7mm-type-model-nfp-c0827 |
| Precision Microdrives 308-100 | Ø8 × 3,4 mm | 1,13 G | 94 mA max. nominal ; 124 mA démarrage max. | 3 V | https://precisionmicrodrives.com/products/8559e4a6-659c-4b32-a622-972adf535f16/308-100-8mm-vibration-motor-3mm-type |
| Precision Microdrives 310-118.001 | Ø10 × 2,1 mm | 1,1 G | 90 mA max. nominal ; 120 mA démarrage max. | 3 V | https://www.precisionmicrodrives.com/product/310-118-001-10mm-vibration-motor-2mm-type |
| NFP-C1030L | Ø10 × 3,0 mm | 1,4 G | 80 mA max. | 3 V | https://precisionminidrives.com/product/10mm-coin-vibration-motor-3mm-type-model-nfp-c1030 |

**Précaution métrologique :** les accélérations normalisées en G issues de fabricants différents ne sont pas nécessairement comparables sans connaître les masses de référence, modes de fixation et méthodes de mesure. Une accélération G élevée n'implique pas automatiquement une meilleure sensation sur la pulpe d'un doigt.

## Arrêt et freinage
- PMD 310-118.001 : temps montée ~96 ms ; arrêt libre ~128 ms ; freinage actif ~50 ms (conditions d'essai fabricant).
- PMD 308-100 : montée ~92 ms ; arrêt libre ~147 ms ; freinage actif ~46 ms.
- Le DRV2605L propose sur les ERM un pilotage en boucle fermée avec surcommande initiale et freinage actif. Fiche TI : https://www.ti.com/product/DRV2605L ; datasheet : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- **Un MOSFET côté masse seul ne garantit pas le freinage actif.** Il faut une topologie de puissance/commande capable d'appliquer le freinage prévu, ou un pilote spécialisé. Ne pas confondre diode de roue libre/protection et freinage contrôlé.
- Un moteur annoncé à 3 V doit recevoir une alimentation et des limites conformes à sa fiche technique ; la LiPo 1S peut dépasser 4 V en charge.

## Variante hors présélection : ERM BLDC
NFP-WS0825 (Ø8 × 2,5 mm, 1,0 G+ annoncé) est sans balais ; intérêt durée de vie, mais architecture de commande/compatibilité DRV2605L à **vérifier** avant toute sélection. https://precisionminidrives.com/product/8mm-brushless-coin-vibration-motor-2-5mm-type-model-nfp-ws0825

## Banc de test conseillé
1. Acheter seulement 1 échantillon de chaque géométrie cible : Ø8 × 1,6 (fin), Ø10 × 2,1 (équilibre), Ø10 × 3 (intense), si sources disponibles et frais acceptables.
2. Tester moteurs avec même fixation / support textile / doigt et pressions contrôlées ; ne pas tester avec les cinq moteurs en première phase.
3. Mesurer réponse mécanique (montée et descente), consommation au repos / au démarrage / à régime, bruit et chauffe.
4. Tester reconnaissance de localisations et séquences 80–400 ms avec essais randomisés, en se limitant à des vibrations confortables.
5. Comparer simple MOSFET à moteur équipé d'un DRV2605L, avec bonne configuration de courant/tension et freinage actif.
6. Conserver une option LRA comme groupe comparatif si le budget le permet.

## Pourquoi pas choisir tout de suite ?
Les candidats maximisent différentes contraintes : épaisseur (C0816), finesse + données de freinage (310-118.001), vibration annoncée (C1030). La taille et la pression de contact sur les doigts peuvent inverser leur ordre de préférence. Aucun prix fournisseur n'est enregistré comme devis ; la BOM sera ajustée **après** présélection et coût total livré vérifié.
