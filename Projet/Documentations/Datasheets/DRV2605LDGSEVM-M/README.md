# DRV2605LDGSEVM-M — Documentation de référence TI

**Date d'indexation : 2026-11-10**

Ce dossier rassemble les liens, métadonnées, notes et contrôles relatifs à la mini-board Texas Instruments. Les trois PDF ont été **fournis dans la conversation et examinés** ; leurs copies binaires ne sont **pas encore archivées ici**. TI limite explicitement la reproduction et la republication de ses ressources ; sur ce dépôt public nous conservons pour l'instant liens officiels et notes de recherche, sans redistribuer les documents complets.

## Documents reçus

| Titre transmis | Référence / pages | Usage |
|---|---|---|
| DRV2605LDGS Haptic Driver Mini Board.pdf | **SLOU396**, août 2014, 9 p. | Guide de mini-board, connexion, BOM p. 4, schéma p. 4, routage p. 5 |
| DRV2605LDGSEVM-M_Haptic Control Console GUI (Rev. B).pdf | **SLOU502B**, rév. janvier 2022, 20 p. | HCC, GUI, registres, calibration, USB2ANY nécessaire avec EVM-M |
| DRV2605LDGSEVM-M EU Declaration of Conformity.pdf | Déclaration TI signée 20 août 2018, 2 p. | Déclaration de conformité **RoHS 2011/65/EU**, **pas** une certification de sécurité d'un gant portable |

## Pages importantes du guide SLOU396
- **Page 2** : deux capacités de découplage 1 µF ; emplacements de résistances R1/R2 I²C laissés non assemblés ; broche VIO pour pull-up au niveau logique du MCU ; VREG ne doit pas alimenter de charge externe.
- **Page 4** : circuit et nomenclature constructeur : U1 DRV2605LDGS, C1/C2 = 1 µF / 25 V X5R en 0402, R1/R2 = 3,3 kΩ (non montées), connecteurs non montés. **Figure 4 : schéma de référence**.
- **Page 5** : Figure 5, implantation / routage des couches haut et bas.
- Le module nécessite un MCU et des commandes I²C. Le **USB2ANY** mentionné dans le document HCC concerne **l'utilisation de la GUI TI sur PC**, pas la commande normale depuis un ESP32.

## Références externes
- Produit : https://www.ti.com/tool/DRV2605LDGSEVM-M
- Datasheet puce : https://www.ti.com/lit/ds/symlink/drv2605l.pdf
- Console HCC (documentation et logiciel) : https://www.ti.com/tool/HAPTICS-CONSOLE
- Manuel SLOU396 : suivre le lien User's Guide depuis la page du produit.

## Note de sécurité et licence
La déclaration RoHS concerne la restriction de certaines substances, non la protection électrique, la batterie LiPo ou la compatibilité d'un dispositif porté. Le guide TI précise qu'un EVM est prévu pour évaluation, pas pour être intégré tel quel à un produit fini ; la sécurité du gant doit être validée séparément.

## Fil TI E2E soumis pour analyse
https://e2e.ti.com/f/1/t/1605518

**Statut : inaccessible depuis les outils de consultation utilisés le 10/11/2026. Son contenu n'est pas établi ; aucune conclusion sur un éventuel problème matériel, rappel ou limitation ne peut être tirée du seul lien.** Obtenir le texte ou une capture pour analyse avant de changer la sélection du DRV2605L.
