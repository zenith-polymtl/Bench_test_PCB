# PDB du banc d'essai Zénith — revue avant commande

Version 3 du 17 novembre 2026, après les dernières réponses de Fey. Ouvrir `PCB/BT_PDB_Alim.PrjPcb`. Les librairies sont dans `PCB/Libs/` et le modèle 3D dans `CAD/BT_PDB_Alim.step`.

## Ce qui a changé depuis la v2

La carte elle-même ne change pas : mêmes 24 composants, même schéma, même routage. Les réponses de Fey et la spec sheet règlent les questions ouvertes :

- **BEC :** Holybro UBEC 5A (3–14S), réglé à 5,2 V. Il donne 5 A en continu et 10 A en pointe, avec ses propres protections contre la surintensité, le court-circuit et la surchauffe. Le budget de courant passe donc (voir plus bas).
- **Tension d'entrée :** l'équipe ne veut que du 5,2 V. On garde la TVS D1 sur l'entrée, sans ajouter d'eFuse.
- **Deux modules ADS1256 :** la nouvelle cellule de charge 6 axes demande deux modules ADS (5 V, 25 mA chacun). Ils sont alimentés tous les deux par J5, avec un dédoublement (Y) dans le faisceau. Ce faisceau est de toute façon fait sur mesure, puisque le bus SPI va aux deux modules. Au total, 50 mA passent par J5, pour 1 A permis par le connecteur.

## La carte

- 35 × 35 mm, FR-4 de 1,6 mm, deux couches de cuivre 1 oz, coins arrondis de 1 mm.
- Les 24 composants sont sur le dessus. Les deux XT30 dépassent du bord : environ 44,5 mm d'un bout à l'autre, sans les câbles. C1 fait 5,9 mm de haut.
- Dessus : polygone +5V, avec J1 relié à J2 en 2,5 mm. Dessous : plan GND.
- Deux trous M3 de 3,2 mm en (3,3 ; 3,3) et (31,7 ; 31,7) mm depuis le coin inférieur gauche.

| Repère | Connexion | Broche 1 | Broche 2 |
|---|---|---|---|
| J1 | Entrée BEC, XT30 mâle | GND | +5V |
| J2 | Raspberry Pi 4, XT30 femelle | GND | +5V |
| J3 | Teensy, JST GH | +5V | GND |
| J4 | Capteur de poids, JST GH | +5V | GND |
| J5 | Modules ADS1256 (×2, par Y), JST GH | +5V_ADC (5 V filtré) | GND |
| J6 | Vibration (IMU ICM-42688-P), JST GH | +3V3_VIB | GND |
| J7 | Radio nRF24L01, JST GH | +3V3_RF | GND |
| J8 | Extra, JST GH | +5V | GND |

## Budget de courant (spec sheet)

| Charge | Courant au pire |
|---|---|
| Raspberry Pi 4 | 3 A |
| Teensy 4.1 | 0,3 A |
| Radio nRF24L01 (par U3) | 0,115 A |
| 2 modules ADS1256 | 0,05 A |
| Cellule de charge | 0,01 A |
| IMU de vibration (par U2) | 0,001 A |
| Sortie extra (prévue) | 1 A |
| **Total** | **environ 4,5 A** |

Le BEC donne 5 A en continu : la marge est d'environ 0,5 A, même avec 1 A sur la sortie extra. Les deux sorties 3,3 V passent par des LDO, dont le courant d'entrée est égal au courant de sortie : elles comptent donc telles quelles sur le 5 V.

## Choix faits

- **Deux LDO 3,3 V au lieu de bucks :** U2 (AP2112K-3.3) pour la vibration et U3 (AMS1117-3.3, avec 22 µF au tantale en sortie comme le demande sa fiche) pour la radio. Un LDO fait moins de bruit qu'un buck et demande beaucoup moins de composants. U3 dissipe environ 0,2 W à 115 mA.
- **Rail ADC :** ferrite 600 Ω à 100 MHz (BLM18AG601, 500 mA) suivie de 12 µF de découplage.
- **Raspberry Pi sur un XT30 :** il peut tirer 3 A, alors qu'un contact JST GH est limité à environ 1 A.
- **Entrée :** condensateur polymère de 220 µF et TVS SMBJ6.0A. La TVS absorbe les pics courts. Elle ne protège pas contre un BEC mal réglé (8 ou 12 V) : le BEC doit rester à 5,2 V.
- **Coût :** environ 6,28 USD de composants par carte aux premiers paliers LCSC, et environ 11,39 USD avec les quantités minimales, sans le PCB, l'assemblage ni le transport (voir `BOM_PDB.xlsx`).

## Vérifications faites dans Altium 26.10.1

- Validate PCB Project : 0 erreur.
- Schéma et PCB synchronisés.
- DRC : 0 violation, 0 avertissement.

## À valider avant de commander

1. **Écarts au cahier des charges :** LDO au lieu de bucks, 35 × 35 mm au lieu de 20 × 20 mm, C1 de 5,9 mm de haut (plus de 5 mm), et J5 en 5 V au lieu de 3,3 V, à la demande du module.
2. **Connecteurs :** vérifier les genres des XT30 avec le BEC, prévoir le câble XT30 → Pi, et respecter la polarité broche 1 = V+ sur les faisceaux GH. Les sorties 5 V et 3,3 V utilisent le même connecteur GH : étiqueter les câbles (la sérigraphie indique déjà la tension de chaque sortie).
3. **Fixation :** position des deux trous M3 à valider avec le châssis.
4. **Premier essai :** alimenter d'abord sur une alimentation de labo limitée en courant, mesurer les rails 5 V, 3,3 V et ADC, puis charger progressivement.
5. Options pour une prochaine version : des points de test (5 V, ADC, 3,3 V, GND) et un LDO plus récent pour la radio.

Fiches : [AMS1117](https://datasheet.lcsc.com/datasheet/pdf/e6935943fc6b1bbf350a1a0f3e90dc4a.pdf?productCode=C6186), [AP2112K](https://datasheet.lcsc.com/lcsc/1809192242_Diodes-Incorporated-AP2112K-3-3TRG1_C51118.pdf), [XT30 mâle](https://datasheet.lcsc.com/datasheet/pdf/f40c503d729038bd14858056d73ccb74.pdf?productCode=C431092), [XT30 femelle](https://datasheet.lcsc.com/datasheet/pdf/8e4f5fbe132d96575dda54e56e670896.pdf?productCode=C2913282), [JST GH](https://datasheet.lcsc.com/datasheet/pdf/145d85af57fc54ac5a4f36dce2b36dd3.pdf?productCode=C189893), [UBEC 5A Holybro](https://holybro.com/products/ubec-5a-3-14s).
