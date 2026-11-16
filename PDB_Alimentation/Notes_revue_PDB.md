# PDB du banc d'essai Zénith — revue avant commande

Version 2 du 16 novembre 2026, après les réponses de Fey. Ouvrir `PCB/BT_PDB_Alim.PrjPcb`. Les librairies sont dans `PCB/Libs/` et le modèle 3D dans `CAD/BT_PDB_Alim.step`.

## Ce qui a changé depuis la v1

- **J5 (ADS1256) passe en 5 V.** Le module ADS utilisé demande 5 V. J5 reçoit maintenant `+5V_ADC` : le 5 V filtré par la ferrite FB1, avec C3 (1 µF), C4 (1 µF) et C5 (10 µF) sur ce rail. Le régulateur U1 (AP2112K) est retiré et la sérigraphie indique « ADS 5V ».
- **Radio :** le nRF tire au plus 115 mA. U3 (AMS1117) dissipe alors environ 0,2 W, ce qui ne pose pas de problème thermique.
- La carte passe de 25 à 24 composants.

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
| J5 | Module ADS1256, JST GH | +5V_ADC (5 V filtré) | GND |
| J6 | Vibration, JST GH | +3V3_VIB | GND |
| J7 | Radio nRF, JST GH | +3V3_RF | GND |
| J8 | Extra, JST GH | +5V | GND |

## Choix faits

- **Deux LDO 3,3 V au lieu de bucks :** U2 (AP2112K-3.3) pour la vibration et U3 (AMS1117-3.3, avec 22 µF au tantale en sortie comme le demande sa fiche) pour la radio. Un LDO fait moins de bruit qu'un buck et demande beaucoup moins de composants.
- **Rail ADC :** ferrite 600 Ω à 100 MHz (BLM18AG601, 500 mA) suivie de 12 µF de découplage.
- **Raspberry Pi sur un XT30 :** il peut tirer 3 A, alors qu'un contact JST GH est limité à environ 1 A.
- **Entrée :** condensateur polymère de 220 µF et TVS SMBJ6.0A.
- **Coût :** environ 6,28 USD de composants par carte aux premiers paliers LCSC, et environ 11,39 USD avec les quantités minimales, sans le PCB, l'assemblage ni le transport.

## Vérifications faites dans Altium 26.10.1

- Validate PCB Project : 0 erreur.
- Schéma et PCB synchronisés (ECO du 16 novembre : retrait de U1, fusion des nets du rail ADC en `+5V_ADC`).
- DRC : 0 violation, 0 avertissement.

## À valider avant de commander

1. **Courant total.** Le pire cas fait environ 4,4 A : Pi 3 A, extra 1 A, Teensy 0,1 A, poids 0,05 A, module ADS environ 0,05 A (à confirmer), vibration et radio environ 0,2 A. Le BEC est donné pour 4 A nominal et 5 A max. Ça passe tant que le Pi et la sortie extra ne tirent pas leur maximum en même temps. À mesurer au premier essai.
2. **Écarts au cahier des charges :** LDO au lieu de bucks, 35 × 35 mm au lieu de 20 × 20 mm, C1 de 5,9 mm de haut (plus de 5 mm), et J5 en 5 V au lieu de 3,3 V, à la demande du module.
3. **Connecteurs :** vérifier les genres des XT30 avec le BEC, prévoir le câble XT30 → Pi, et respecter la polarité broche 1 = V+ sur les faisceaux GH.
4. **Fixation :** position des deux trous M3 à valider avec le châssis.
5. Le repère U1 n'est plus utilisé. U2 et U3 gardent leur nom.

Fiches : [AMS1117](https://datasheet.lcsc.com/datasheet/pdf/e6935943fc6b1bbf350a1a0f3e90dc4a.pdf?productCode=C6186), [AP2112K](https://datasheet.lcsc.com/lcsc/1809192242_Diodes-Incorporated-AP2112K-3-3TRG1_C51118.pdf), [XT30 mâle](https://datasheet.lcsc.com/datasheet/pdf/f40c503d729038bd14858056d73ccb74.pdf?productCode=C431092), [XT30 femelle](https://datasheet.lcsc.com/datasheet/pdf/8e4f5fbe132d96575dda54e56e670896.pdf?productCode=C2913282), [JST GH](https://datasheet.lcsc.com/datasheet/pdf/145d85af57fc54ac5a4f36dce2b36dd3.pdf?productCode=C189893).
