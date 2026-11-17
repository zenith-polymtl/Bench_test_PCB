# Multiconnecteur du banc d'essai Zénith — revue avant commande

Version 3 du 17 novembre 2026, après les dernières réponses de Fey. Ouvrir `PCB/BT_Multiconnecteur.PrjPcb`. Les librairies sont dans `PCB/Libs/` et le modèle 3D dans `CAD/BT_Multiconnecteur.step`.

## Ce qui a changé depuis la v2

- **Pistes MR30 élargies :** 1,45 mm → 2,5 mm, sur les deux faces, pour les trois phases et des deux côtés. Elles peuvent maintenant porter les 15 A du connecteur.
- **Piste de la phase A éloignée du bord :** son tronçon vertical passe de 1,4 à 1,9 mm du bord gauche (et de 88,6 à 88,1 mm à droite). Il reste 0,65 mm entre le cuivre et le bord de la carte.
- **Sérigraphie « MR30 15A MAX »** à côté de J2 (dessus) et de J7 (dessous, en miroir).
- Polygones recoulés après la modification. Rien d'autre ne change : mêmes composants, même schéma, même modèle 3D.
- **Nouvelle BOM :** `BOM_Multiconnecteur.xlsx` (références, fournisseurs et prix des connecteurs).

## La carte

- 90 × 60 mm, deux couches de cuivre 2 oz (70 µm).
- Trois nets : PHASE_A, PHASE_B et PHASE_C. Côté ESC à gauche, côté moteur à droite.
- Chaque phase a un polygone sur le dessus et un sur le dessous, reliés par 18 vias.
- Les connecteurs d'un même côté sont en parallèle. Une phase alimentée est donc aussi présente sur les connecteurs inutilisés (capuchons à prévoir).
- Quatre trous M3 (3,2 mm, non métallisés) en (4 ; 56), (86 ; 56), (22 ; 4) et (68 ; 4) mm depuis le coin inférieur gauche. Positions provisoires.

| Connexion | Côté ESC | Côté moteur | Affectation |
|---|---|---|---|
| WAGO 2716-103 (levier) | J1, monté dessous | J6, dessus | L1.x = A, L2.x = B, L3.x = C |
| Amass MR30 coudé | J2, mâle, dessus | J7, femelle, monté dessous | 1 = A, 2 = B, 3 = C, ergots MH sans net |
| Bullets 5,5 mm | J3, J4, J5 (mâles) | J8, J9, J10 (femelles) | A, B, C de haut en bas |

J1 et J7 sont sous la carte pour garder l'ordre A-B-C aligné des deux côtés sans croiser les phases. La carte se monte donc sur entretoises, avec un accès par en dessous. Le WAGO J1 descend de 28,8 mm sous la carte : les entretoises doivent faire au moins 30 mm.

## Tenue en courant

### Le courant dans chaque phase

D'après Fey, les 80 à 84 A sont le courant du moteur. Le moteur n'a pas de fil de retour : à chaque instant, le courant entre par une phase et ressort par une autre. Il ne se partage donc pas en trois.

- **Courant crête dans une phase :** jusqu'à environ 80 A, quand le moteur tire 80 A.
- **Courant efficace (RMS) dans une phase :** environ 0,82 × 80 ≈ 65 A. En commande 6 pas, chaque phase conduit 4 pas sur 6, d'où le facteur √(4/6). Une commande sinusoïdale à plein régime donne un ordre de grandeur semblable.
- Le calcul « 80 A ÷ 3 = 27 A » sous-estime donc la chaleur dans les pistes d'un facteur d'environ 6.

La carte a été vérifiée pour 65 A efficaces par phase sur le chemin principal (WAGO et bullets).

### Vérification

| Élément | Limite | À 65 A efficaces | Verdict |
|---|---|---|---|
| Polygones (passage le plus étroit : 7,5 mm sur chaque face, 2 oz) | — | environ +44 °C en continu selon l'IPC-2221 (+6 °C à 27 A) | Acceptable pour le FR-4, moins en pointe de quelques secondes |
| WAGO 2716-103 | 76 A (IEC), 55 A (UL) | Sous la limite IEC, au-dessus de la limite UL en continu | Correct pour des essais en pointe ; à surveiller en continu |
| Bullets 5,5 mm | selon le fabricant | — | Dépend des vrais connecteurs (voir plus bas) |
| MR30 (J2, J7) | 15 A en continu | Non prévu pour ce courant | Réservé aux moteurs de 15 A par phase au plus |

- **Pistes MR30 :** à 15 A, les nouvelles pistes de 2,5 mm sur les deux faces chauffent d'environ +10 °C (contre +23 °C avec les anciennes de 1,45 mm).
- L'IPC-2221 est prudente pour des passages courts reliés à de gros connecteurs. Une mesure thermique au premier essai reste recommandée : courant progressif, sans moteur, avec un thermomètre infrarouge ou une caméra thermique.

## Vérifications faites dans Altium 26.10.1

- Polygones recoulés (Repour All).
- DRC du 17 novembre : 0 violation, 0 avertissement.
- Schéma inchangé depuis la v2.

## À valider avant de commander

1. **Bullets :** mesurer nos vrais bullets au pied à coulisse. Le perçage de 7,2 mm vient d'une fiche générique.
2. **Fixation :** positions finales des trous M3 selon le nouveau montage du banc, et entretoises d'au moins 30 mm.
3. **Genres réels** des bullets et des MR30 avec les câbles de l'ESC et des moteurs, et capuchons pour les connecteurs inutilisés.
4. **Commande :** préciser du cuivre 2 oz sur les deux faces. En 1 oz, les calculs ci-dessus ne tiennent plus.
5. **Approvisionnement :** le WAGO 2716-103 (2 par carte, environ 21 $ CA l'unité) est en rupture chez DigiKey Canada le 17 novembre, avec 40 pièces attendues le 22 décembre 2026. À commander tôt ou chez un autre distributeur.
6. Petits détails pour plus tard : l'empreinte des bullets s'appelle encore `_5.5`, et la règle de vias de l'éditeur de règles (1,27 / 0,71 mm) ne correspond pas aux vias réels (0,90 / 0,45 mm).

Fiche du bornier WAGO 2716-103 : https://www.wago.com/2716-103
