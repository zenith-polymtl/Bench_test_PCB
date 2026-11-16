# Multiconnecteur du banc d'essai Zénith — revue avant commande

Version 2 du 16 novembre 2026, après les réponses de Fey. Ouvrir `PCB/BT_Multiconnecteur.PrjPcb`. Les librairies sont dans `PCB/Libs/` et le modèle 3D dans `CAD/BT_Multiconnecteur.step`.

## Ce qui a changé depuis la v1

- **Quatre trous de fixation M3** (3,2 mm, non métallisés) en (4 ; 56), (86 ; 56), (22 ; 4) et (68 ; 4) mm depuis le coin inférieur gauche. Les coins du bas sont occupés par les MR30, alors les deux trous du bas sont décalés vers le centre. Ces positions sont provisoires, puisque la fixation du banc va changer.
- **Courant confirmé :** 80 A / 3, soit au plus 26,7 A par fil et par connexion. La section « Tenue en courant » est mise à jour en conséquence.

## La carte

- 90 × 60 mm, deux couches de cuivre 2 oz (70 µm).
- Trois nets : PHASE_A, PHASE_B et PHASE_C. Côté ESC à gauche, côté moteur à droite.
- Chaque phase a un polygone sur le dessus et un sur le dessous, reliés par 18 vias.
- Les connecteurs d'un même côté sont en parallèle. Une phase alimentée est donc aussi présente sur les connecteurs inutilisés (capuchons à prévoir).

| Connexion | Côté ESC | Côté moteur | Affectation |
|---|---|---|---|
| WAGO 2716-103 (levier) | J1, monté dessous | J6, dessus | L1.x = A, L2.x = B, L3.x = C |
| Amass MR30 coudé | J2, mâle, dessus | J7, femelle, monté dessous | 1 = A, 2 = B, 3 = C, ergots MH sans net |
| Bullets 5,5 mm | J3, J4, J5 (mâles) | J8, J9, J10 (femelles) | A, B, C de haut en bas |

J1 et J7 sont sous la carte pour garder l'ordre A-B-C aligné des deux côtés sans croiser les phases. La carte se monte donc sur entretoises, avec un accès par en dessous. Le WAGO J1 descend de 28,8 mm sous la carte : les entretoises doivent faire au moins 30 mm.

## Tenue en courant (26,7 A par phase)

- **Polygones :** le passage le plus étroit fait 7,5 mm sur chaque face, en 2 oz, avec les deux faces en parallèle. D'après l'IPC-2221, l'échauffement est d'environ +10 °C à 26,7 A.
- **WAGO 2716-103 :** donné pour 76 A (IEC) et 55 A (UL), donc largement suffisant.
- **Bullets 5,5 mm :** suffisants pour ce courant.
- **MR30 :** donné pour 15 A en continu (30 A pendant 1 min). Le chemin MR30 reste réservé aux moteurs qui tirent au plus 15 A par phase. Ses pistes (1,45 mm sur les deux faces) tiennent environ 10 à 14 A ; il faudrait les élargir à environ 2,5 mm pour profiter des 15 A du connecteur.

## Vérifications faites dans Altium 26.10.1

- Validate PCB Project : 0 erreur.
- Schéma et PCB synchronisés.
- DRC : 0 violation, 0 avertissement, avec les trous de fixation.

## À valider avant de commander

1. **Bullets :** mesurer nos vrais bullets au pied à coulisse. Le perçage de 7,2 mm vient d'une fiche générique.
2. **Fixation :** positions finales des trous M3 selon le nouveau montage du banc, et entretoises d'au moins 30 mm.
3. **MR30 :** confirmer qu'on se limite aux petits moteurs sur ce chemin, ou élargir ses pistes.
4. **Genres réels** des bullets et des MR30 avec les câbles de l'ESC et des moteurs, et capuchons pour les connecteurs inutilisés.
5. Petit détail : l'empreinte des bullets s'appelle encore `_5.5`, à renommer.

Fiche du bornier WAGO 2716-103 : https://www.wago.com/2716-103
