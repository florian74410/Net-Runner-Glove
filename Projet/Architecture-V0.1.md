# Architecture NRP — hypothèse V0.1

```text
[Smartphone / PC / application]
            |
           BLE
            |
[Bracelet : ESP32 + batterie protégée + charge + régulation]
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

## Arbitrages en suspens
- ERM + MOSFET/PWM vs LRA + pilotes dédiés DRV2605L.
- Contacts binaires vs capteurs analogiques de pression.
- 5 GPIO indépendants vs multiplexage des entrées ; I²C partagé pour les pilotes, sans confondre avec multiplexage de puissance.
- Alimentation moteurs adaptée à leur tension nominale, sécurités LiPo, gestion des pointes.
- Électronique miniaturisée sans sacrifier ergonomie ni réparabilité.

## Hors périmètre initial
IMU/gyroscope, suivi 3D stable, casque VR, deux gants et duplex complet ; ces éléments pourront découler d'essais concluants.
