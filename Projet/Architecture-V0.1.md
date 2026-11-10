# Architecture NRP — hypothèse V0.1

```text
[Smartphone / PC / application]
            |
           BLE
            |
[Bracelet : ESP32 + rail régulé 3,3 V + protection batterie]
[Bloc batterie LiPo 1S amovible : recharge séparée hors gant]
            |
     [Pilotes moteurs + lecture capteurs]
            |
  [Faisceau souple et connecteurs démontables]
            |
[Doigts : 5 haptiques + jusqu'à 5 contacts]
```

## Interfaces
- Machine → humain : motifs haptiques localisés sur 5 doigts.
- Humain → machine : capteurs contact/pression (prototype simple au départ).
- Langage logique NRP : identité des symboles, durée, intensité éventuelle, rythme, localisation, erreurs, version du protocole.
- Dictionnaires par usage (WEB / GUI / 3D) : extension future, avec vocabulaire de base cohérent.

## Conception physique
Électronique concentrée dans le bracelet ; doigt = seulement actionneur/capteur, support textile ou embout amovible, câble longeant la main comme un tendon. Modules remplaçables indépendamment. Configuration main gauche / main droite prévue au niveau logiciel.

## Alimentation et sécurité — exigences V0.1
- Rail d'alimentation commun **3,3 V régulé** pour MCU, pilotes, logique et capteurs, depuis batterie LiPo 1S protégée et convertisseur buck-boost adapté aux pointes.
- Le bloc batterie est **amovible**, mécaniquement isolé et maintenu à l'écart des articulations ; **recharge hors du gant** via chargeur adapté avec surveillance thermique.
- Protection **matérielle** contre surcharge, décharge profonde, surintensité et court-circuit ; aucune sécurité essentielle ne dépend du seul logiciel.
- Télémétrie logicielle (tension/température/alertes/arrêt) comme protection complémentaire.
- Prototype testé initialement sur alimentation de laboratoire limitée en courant ; vérification des températures et des modes de défaillance avant port.
- Les moteurs restent commandés selon leurs limites de tension, malgré le bus 3,3 V. [Étude E003 — alimentation et sécurité](Etudes/E003-Alimentation-3V3.md).

## Arbitrages en suspens
- ERM + MOSFET/PWM vs LRA + pilotes dédiés DRV2605L.
- Contacts binaires vs capteurs analogiques de pression.
- 5 GPIO indépendants vs multiplexage des entrées ; I²C partagé pour les pilotes, sans confondre avec multiplexage de puissance.
- Alimentation moteurs adaptée à leur tension nominale, sécurités LiPo, gestion des pointes.
- Électronique miniaturisée sans sacrifier ergonomie ni réparabilité.

## Hors périmètre initial
IMU/gyroscope, suivi 3D stable, casque VR, deux gants et duplex complet ; ces éléments pourront découler d'essais concluants.
