# PDB du banc d’essai Zénith — revue avant commande

## La carte

Je distribue les 5 V du BEC sur un PCB de **35 × 35 mm**, en FR-4 de 1,6 mm, avec deux couches de cuivre de 1 oz et quatre coins de rayon 1 mm. J’ai placé les 25 composants sur le dessus. Les deux XT30 dépassent du PCB. D’après leurs contours d’implantation, l’ensemble occupe environ **44,5 mm** entre leurs extrémités, avant branchement des câbles. Le condensateur d’entrée mesure déjà 5,9 mm au-dessus du PCB.

| Repère | Connexion | Broche 1 | Broche 2 |
|---|---|---|---|
| J1 | Entrée BEC, XT30 mâle | GND | +5V |
| J2 | Raspberry Pi 4, XT30 femelle | GND | +5V |
| J3 | Teensy, JST GH | +5V | GND |
| J4 | Capteur de poids, JST GH | +5V | GND |
| J5 | Module ADS1256, JST GH | +3V3_ADC | GND |
| J6 | Vibration, JST GH | +3V3_VIB | GND |
| J7 | Radio, JST GH | +3V3_RF | GND |
| J8 | Extra, JST GH | +5V | GND |

## Choix faits

J’ai relié J1 à J2 par une piste de 2,5 mm sur le dessus, dans le polygone +5V. J’ai réservé le dessous à GND et placé six vias de masse près de chaque XT30. J’ai marqué le contact positif de chaque connecteur. Le nom de la carte est sérigraphié au dos.

J’ai choisi trois LDO. Pour l’ADC, FB1 alimente l’entrée et EN de U1, avec 1 µF après la ferrite. U2 alimente la vibration. Sur la radio, la broche 2 et la languette de U3 rejoignent le cuivre +3V3_RF. J’ai retenu **22 µF au tantale** en sortie de l’AMS1117 conformément à sa fiche. Les cinq rails de distribution et les deux liaisons locales de la ferrite et de la LED figurent dans la netlist. [Fiche AMS1117](https://datasheet.lcsc.com/datasheet/pdf/e6935943fc6b1bbf350a1a0f3e90dc4a.pdf?productCode=C6186)

J’ai chiffré les composants à **6,45 USD par carte** aux premiers paliers LCSC. Les quantités minimales peuvent porter l’achat initial à environ **11,39 USD**, avant transport, taxes, PCB et assemblage. Si le budget de 15 $ est en CAD, le coût monté des composants reste sous ce seuil tant que 1 USD vaut moins de 2,32 CAD.

## À valider avant de commander

- Je demande l’accord sur trois écarts au cahier : les LDO à la place des bucks, l’utilisation de CI et le dépassement de 20 × 20 × 5 mm. Je voudrais valider les M3 de 3,2 mm en (3,3 ; 3,3) et (31,7 ; 31,7) mm, l’espace sous les pattes traversantes et le passage des câbles.
- Je veux essayer les genres des deux XT30 avec le BEC et le câble **XT30 → Pi**. Je veux contrôler au multimètre la continuité jusqu’au Pi et la convention **broche 1 = V+** sur les six faisceaux GH. Les dessins Amass indiquent les cotes et le détrompage ; je confirmerai la correspondance des signes moulés avec les pads.
- Mon scénario de pointe donne **4,75 A** : Pi 3 A, Teensy 0,10 A, poids 0,05 A, extra 1 A et trois rails 3,3 V totalisant 0,60 A. Il faut ajouter la LED et les courants propres des régulateurs. Je veux mesurer la durée des pointes admissible pour le BEC de 4 A nominal / 5 A max, la chute de tension au Pi et les transitoires lors des changements de régime des ESC. La TVS SMBJ6.0A peut écrêter jusqu’à 10,3 V au courant d’essai spécifié ; elle ne garantit pas 5,3 V aux charges.
- Je dois connaître les pointes et le courant soutenu de la radio. Si elle prend à elle seule 0,60 A à 5,3 V d’entrée, U3 dissipe environ **1,2 W**. Je veux mesurer sa température ainsi que le bruit et la stabilité des trois sorties sous charge. La ferrite du rail ADC est spécifiée à 500 mA.
- Je dois confirmer que **le module ADS1256** accepte une alimentation unique de 3,3 V sur J5. La puce nue demande aussi 4,75 à 5,25 V sur AVDD. Je veux vérifier la tension admissible de chaque appareil relié directement au BEC. [Fiche TI](https://www.ti.com/product/ADS1256)
- J’ai contrôlé les fichiers CFB, les 55 broches logiques câblées et la correspondance des 25 composants. Le contrôle indépendant du PCB importé dans KiCad 9, avec les règles d’espacement du projet, donne zéro violation et zéro pad non connecté avant et après recalcul du cuivre. Je garde la validation du projet, la revue de l’ECO et le DRC **dans Altium 26.10.1** pour la dernière étape.

Dessins consultés : [XT30 mâle](https://datasheet.lcsc.com/datasheet/pdf/f40c503d729038bd14858056d73ccb74.pdf?productCode=C431092), [XT30 femelle](https://datasheet.lcsc.com/datasheet/pdf/8e4f5fbe132d96575dda54e56e670896.pdf?productCode=C2913282), [JST GH](https://datasheet.lcsc.com/datasheet/pdf/145d85af57fc54ac5a4f36dce2b36dd3.pdf?productCode=C189893), [AP2112K](https://datasheet.lcsc.com/lcsc/1809192242_Diodes-Incorporated-AP2112K-3-3TRG1_C51118.pdf). Les empreintes reprennent les plages de cuivre des bibliothèques KiCad et des boîtiers métriques nominaux, avec des coins de pads CMS rectangulaires. Je confirmerai l’ajustement des pièces et des perçages à l’échelle 1:1.
