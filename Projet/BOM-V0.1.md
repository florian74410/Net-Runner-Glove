# BOM prototype NRP V0.1 — simulation, non commande

Budget cible **100 € TTC**. Prix indicatifs issus d'une première conversation, **non revalidés dans ce dépôt**. Prix finaux, taxes, transport et compatibilités à vérifier avant toute commande.

| Poste | Quantité / hypothèse | Estimation (€) |
|---|---|---:|
| ESP32-C3 Mini (BLE) | 1 | 10,99 |
| Mini moteurs ERM | lot de 10, 5 utilisés | 7,85 |
| Contacts de doigts simples | 5 | 3,25 |
| MOSFET + résistances/protections | pour 5 canaux | 3,25 |
| Batterie LiPo 1S protégée | 1 000 mAh, 1 | 12,00 |
| Module charge USB-C adapté | 1 | 3,50 |
| Régulateur 3,3 V | 1 | 5,00 |
| Connecteurs débrochables | ensemble | 5,00 |
| Câbles et gaines flexibles | ensemble | 6,00 |
| Textile, attaches, supports | ensemble | 8,00 |
| Interrupteur et support de montage | ensemble | 4,00 |
| Transport prévisionnel | provision | 7,00 |
| **Sous-total** | | **75,84** |
| **Imprévus 15 %** | | **11,38** |
| **Total ECO simulé** | | **87,22** |

## Option HAPTIC
Cinq pilotes DRV2605L et éventuellement multiplexeur I²C (si adresses identiques) : budget antérieurement estimé à **138,08 €** sur modules de développement ; **estimation à revalider**, pas une BOM professionnelle. Un pilote DRV2605L commande un actionneur ERM ou LRA ; le multiplexage I²C ne remplace pas les étages de puissance.

## Hypothèses d'énergie (non mesurées)
- Batterie nominale : 3,7 V / 1 000 mAh (~3,7 Wh).
- Capacité exploitable prise au modèle : 80 % ; rendement global supposé 85 %.
- ESP32 + BLE : 35 mA @ 3,3 V ; auxiliaires : 5 mA @ 3,3 V.
- ERM : 80 mA @ 3 V par moteur, actif.
- Modèle : P_moy = (I_MCU + I_aux) × 3,3 V + N_moteurs × taux_activité × I_moteur × 3 V.
- Courant batterie estimé : P_moy / (3,7 V × 0,85).
- Autonomie estimée : capacité_mAh × 0,8 / courant_batterie_mA.
- **Ne pas déduire la taille minimale de batterie de cette seule moyenne : vérifier courants de démarrage, courant de pointe, câblage, charge et limites thermiques.**

## Politique d'achat
1. Comparer références exactes de moteurs et capteurs (dimensions, durée de vie, vibrations, consommation).
2. Comparer carte ESP32 compacte, régulateur, batterie protégée et connecteurs détachables.
3. Évaluer pilotes LRA/ERM miniatures et coût total des circuits nus vs modules Adafruit.
4. Ne pas commander avant validation des tensions/courants compatibles et du schéma électrique.
5. Séparer devis prototype (modules) et BOM industrielle (PCB, assemblage, tests, conformité).
