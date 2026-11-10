# Étude E001 — Actionneurs haptiques de gants commerciaux et prototypes

**Date : 2026-11-10** — **Statut : exploration documentée, sans validation expérimentale NRP**.

## Objet
Identifier les familles d'actionneurs réellement employées dans les gants commerciaux et publications, puis sélectionner un protocole de comparaison pertinent pour le langage NRP. Distinguer technologie déclarée (fiable), référence exacte des moteurs (souvent non divulguée), et interprétation de nos besoins (hypothèse).

## Inventaire confirmé

| Système | Technologie annoncée | Répartition | Source |
|---|---|---|---|
| bHaptics TactGlove DK2 | 6 LRA/main | 5 extrémités + point supplémentaire | https://www.bhaptics.com/shop/tactglove |
| bHaptics TactGlove DK3 | 7 LRA haute définition + 1 VCM/main | 8 points/main | https://www.bhaptics.com/shop/buy-tactsuit/tactglove-dk3/ |
| SenseGlove Nova 1 | 2 LRA (pouce, index) + 1 VCA («Thumper» dos de main) | 3 points | https://senseglove.gitlab.io/SenseGloveDocs/nova-glove.html |
| SenseGlove Nova 2 | 4 LRA, environ 180 Hz | pouce et index + 2 dans la paume | https://senseglove.gitlab.io/SenseGloveDocs/nova-2.html |
| MANUS Prime II Haptic | 5 LRA programmables | un par doigt | https://pmc.ncbi.nlm.nih.gov/articles/PMC8070066/ |
| HaptX Gloves G1 | 135 actionneurs microfluidiques/main + retour d'effort | doigts et paume | https://haptx.com/gloves-g1/ |

Sources supplémentaires : https://www.mdpi.com/1424-8220/21/8/2667 (revue systématique, 2021). **Les marques et modèles exacts des LRA internes ne sont généralement pas fournis publiquement : ne pas inventer de références.**

## Données d'autonomie publiées
- bHaptics DK2 : batterie Li-ion 630 mAh 3,8 V, 3,5 h d'autonomie annoncée ; protocole déclaré : tous les points pendant 1 s chaque 5 s, intensité max. Voir page officielle produit et manuel : https://www.bhaptics.com/shop/tactglove et https://cdn.bhaptics.com/files/tactglove-manual-en.pdf
- bHaptics DK3 : 630 mAh, jusqu'à 4 h annoncées ; ne pas comparer directement avec DK2 tant que les protocoles ne sont pas vérifiés. https://www.bhaptics.com/shop/buy-tactsuit/tactglove-dk3/

## Comparaison des principes

| Famille | Avantages pratiques | Limites / précautions | Priorité NRP |
|---|---|---|---|
| ERM (masse excentrée) | Pilotage très simple pour prototype ; coût généralement bas ; perception pouvant être favorable pour la localisation | démarrage/arrêt et inertie ; usure ; vibrations non directionnelles ; qualité du montage | Comparatif prioritaire |
| LRA (résonateur linéaire) | Miniaturisation, direction de stimulation définie, contrôle fin avec pilote adapté, efficacité possible | alimentation alternative autour de la résonance ; driver spécifique recommandé ; résultats perceptifs non universellement supérieurs | Comparatif prioritaire |
| VCA / VCM (bobine mobile) | Impulsions ou effets plus riches et puissants selon design | coût et intégration variable | Plus tard |
| Piézo / films électroactifs | Faible épaisseur, réponse rapide possible | besoin de tensions/électronique particulières, contrainte d'intégration | R&D ultérieure |
| Microfluidique / pression | Sensation de pression et contact plutôt que simple vibration | compresseur/airpack et système complexe | Hors V0.1 |

Voir : https://www.precisionmicrodrives.com/design-guidance-for-haptic-wearables et https://www.precisionmicrodrives.com/ab-028

## Résultat scientifique IMPORTANT (ne pas généraliser)
*Perception in Hand-Worn Haptics: Placement, Simultaneous Stimuli, and Vibration Motor Comparisons*, Seim et al., Georgia Tech, 2015 : https://repository.gatech.edu/items/37b51455-ed65-4be9-8072-a616270c0633

Résumé de la notice institutionnelle :
- Le placement sur les doigts (côté palmaire, notamment vers la base) influence la reconnaissance ;
- la reconnaissance de plus de deux points simultanés devient difficile dans ces essais ;
- dans cette étude, les ERM ont donné une meilleure reconnaissance de localisation que les LRA dans des configurations testées.

**Cette conclusion ne signifie pas que tous les ERM sont meilleurs que tous les LRA ; le montage, le type précis de moteur, la peau stimulée et la tâche comptent.** Pour NRP, tester surtout *des séquences discrètes* plutôt que des accords de cinq moteurs.

## Fiabilité mécanique
SenseGlove précise que ses LRA de doigts connectés par fil textile conducteur sont moins fiables que les moteurs de paume câblés : https://senseglove.gitlab.io/SenseGloveDocs/nova2-vibration.html . NRP privilégie des liaisons souples, débranchables et réparables ; qualification de la fatigue des fils et des connecteurs indispensable.

## Modèles LRA concrets — pour benchmarking, pas achats décidés
Les chiffres viennent des pages de fabricants et sont **propres au modèle et aux conditions de mesure** :
- Precision Microdrives **C08-00A**, 8 × 2,6 mm ; 240 Hz ; courant typique 28 mA ; temps de montée 27 ms ; arrêt 83 ms. https://www.precisionmicrodrives.com/products/76203c6d-e150-41cd-9a11-58da89caf06e/c08-00a-8mm-coin-lra
- **C08-005**, 8 × 3,2 mm ; 235 Hz ; courant typique 75 mA ; montée 33 ms ; arrêt 123 ms. https://www.precisionmicrodrives.com/products/32931431-4ed1-49ce-870b-5766169ee5c6/c08-005-8mm-coin-lra
- **C10-100**, 10 × 3,7 mm ; 175 Hz ; courant typique 67 mA ; montée 37 ms ; arrêt 177 ms. https://www.precisionmicrodrives.com/products/fb7e332c-199f-4922-ac22-857688f22351/c10-100-10mm-coin-lra

**Attention :** les temps d'arrêt ci-dessus ne doivent pas être confondus avec les performances d'un driver à freinage actif ; ne pas déduire la latence réelle NRP sans mesure.

## Contrôle électronique
Texas Instruments DRV2605L : un driver pour un ERM ou LRA, auto-résonance des LRA, freinage actif, bibliothèque d'effets, commandes I²C, boîtiers CMS petits ; il ne pilote pas cinq moteurs séparément. https://www.ti.com/product/DRV2605L

Un bus I²C ou multiplexeur partage les commandes mais **pas** le courant d'alimentation des actionneurs. La miniaturisation du CI ne garantit pas le coût faible d'un PCB assemblé en une seule unité.

## Protocole comparatif proposé
1. Tester au minimum un **ERM miniature** et un **LRA miniature** de taille similaire avec tension/pilote corrects.
2. Fixation reproductible (même doigt, même pression et orientation), tester côté dorsal/palmaire sans gêner le contact pouce-doigt.
3. Motifs de 50, 100, 200 et 400 ms ; comparer latence ressentie, distinction des séquences, intensité, bruit et confort.
4. Test aveugle 1, 2, puis 5 canaux, événements isolés et deux points simultanés ; mesurer confusions plutôt que supposer 31 symboles utilisables.
5. Mesurer courant veille, courant par effet, pics, énergie par symbole, robustesse de câble au mouvement.
6. Garder priorité à la performance humaine et fiabilité du langage ; pas automatiquement au moteur le moins cher.

## Conclusion temporaire
**NRP ne doit pas sélectionner un LRA uniquement parce que les gants haut de gamme en utilisent.** Tester ERM contre LRA sur notre tâche de communication de symboles. Le choix peut conduire à ERM + MOSFET (simple), ERM + DRV2605L (amélioration d'effet), ou LRA + DRV2605L, selon mesures réelles. Aucun composant définitif arrêté.
