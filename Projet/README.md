# NRP — Projet

**Net-Runner-Glove (NRP)** — recherche et prototypage personnel d'une interface homme-machine tactile, non invasive, réversible et évolutive.

## Objectif principal
Étudier et créer un **langage homme-machine haptique** réutilisable entre plusieurs usages (navigation WEB, interfaces graphiques, 3D/VR à terme). Le gant est le premier support matériel envisagé, **pas une fin en soi**.

## Périmètre V0.1 (hypothèses à tester)
- 1 main pour commencer, avec configuration droitier/gaucher à prévoir.
- 5 actionneurs haptiques au niveau des doigts ; capteurs de contact ou pression sur les doigts.
- Électronique principale, MCU BLE et batterie sur un bracelet/avant-bras.
- Faisceaux flexibles le long des doigts ; modules déconnectables, remplaçables et réparables.
- Communication BLE avec smartphone ou ordinateur ; navigation sur réseaux via logiciel hôte (pas un accès réseau direct supposé du gant).
- Mode bidirectionnel possible à terme, **non requis pour le premier essai**.
- Gyroscope/IMU, suivi spatial, VR et deux mains : pistes futures, non retenues d'office en V0.1.

## Contraintes
- Budget souhaité du premier prototype : **≤ 100 €** (objectif, non coût garanti).
- Priorité à la qualité des sensations, à la sécurité électrique, au confort et à la maintenabilité.
- Ne pas réduire arbitrairement la qualité des capteurs/actionneurs ; optimiser plutôt carte MCU, intégration et architecture.
- Batterie LiPo 1S protégée, charge adaptée, validation des pics de courant et de l'autonomie réelle.
- Aucun achat, choix technique définitif ou affirmation de nouveauté sans validation.

## Fichiers
- [BOM et puissance](BOM/BOM-V0.1.md)
- [Dossier BOM](BOM/README.md)
- [Inventaire des études et antériorités](Etudes/Antecedents-V0.1.md)
- [Développement](Developpement/README.md)
- [Documentations](Documentations/README.md)
- [Architecture de départ](Architecture-V0.1.md)
- [Hypothèses et expériences](Recherche-et-tests.md)
- [Journal de décisions et travail IA](../IA/README.md)

## Statut
Phase de cadrage : accès au FabLab à vérifier, BOM exploratoire à affiner, premiers tests haptiques à définir.

> Dépôt public : éviter d'y publier coordonnées personnelles, données de testeurs, clés BLE, secrets ou idées confidentielles destinées à une éventuelle protection industrielle.
