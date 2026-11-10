# Décisions et justifications

| Date | Statut | Décision / constat | Pourquoi |
|---|---|---|---|
| 2026-11-10 | Confirmé | Le langage haptique NRP est l'objectif principal ; le gant est un support | Permet dictionnaires WEB, GUI, 3D et d'autres appareils |
| 2026-11-10 | Confirmé | Démarrer par un prototype personnel économique | Valider l'intérêt avant de solliciter financement |
| 2026-11-10 | Confirmé | Séparer `Projet/` et `IA/` | Documentation et continuité de développement |
| 2026-11-10 | Orientation | Électronique sur bracelet, capteurs/actionneurs seuls aux doigts | Confort, modularité et maintenance |
| 2026-11-10 | Orientation | Privilégier qualité des actionneurs/capteurs | Cœur de l'expérience tactile |
| 2026-11-10 | À trancher | ERM+MOSFET vs DRV2605L et LRA | Coût, encombrement, fidélité du motif |
| 2026-11-10 | Différé | IMU, VR, deux mains, duplex complet | Non indispensable à V0.1 |
| 2026-11-10 | À vérifier | Accessibilité, nouveauté commerciale et liberté d'exploitation | Recherche d'antériorité non exhaustive |
| 2026-11-10 | Confirmé (objectif de conception) | Rail 3,3 V régulé commun ; LiPo 1S protégée via buck-boost | Cohérence des niveaux électriques ; batterie brute ≠ 3,3 V |
| 2026-11-10 | Confirmé (exigence de sécurité) | Protection matérielle LiPo, contrôleur de charge adapté, isolation physique et surveillance logicielle complémentaire | Dispositif porté : la sécurité prime sur coût, taille et autonomie |
| 2026-11-10 | Confirmé pour V0.1 | Batterie amovible, rechargée hors du gant ; pas de port pendant charge | Réduire le risque lié à la charge au contact du corps |
| 2026-11-10 | Confirmé pour les premiers essais | Alimentation de laboratoire limitée en courant avant intégration batterie | Vérifier fonctionnement, pics, température et défauts |
