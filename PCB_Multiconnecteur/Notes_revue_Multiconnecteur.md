# Multiconnecteur du banc d’essai Zénith
Version pour revue du 15 novembre 2026. Ouvrir `PCB/BT_Multiconnecteur.PrjPcb`. Les documents et les empreintes utilisés sont inclus. Le projet conserve les références au dépôt commun de l’équipe dans son emplacement habituel.

## Connecteurs et brochage

| Connexion | Côté ESC | Côté moteur | Affectation |
|---|---|---|---|
| WAGO 2716-103 | J1, dessous | J6, dessus | Les quatre contacts L1.x rejoignent A, L2.x rejoignent B et L3.x rejoignent C |
| Amass MR30 coudé | J2, mâle, dessus | J7, femelle, dessous | 1 = A, 2 = B, 3 = C. Les deux ergots MH sont isolés |
| Bullets 5,5 mm, dessus | J3, J4, J5, mâles proposés | J8, J9, J10, femelles proposées | A, B, C dans l’ordre indiqué |

Les connecteurs d’un même côté sont câblés en parallèle. Une phase alimentée est présente également sur ses connecteurs inutilisés. Les genres réels et les capuchons doivent être confirmés par l’équipe.

## Vérifications effectuées

- Relecture des symboles et du PCB avec un importeur indépendant. Comparaison du schéma avec les pads du PCB après import. Les 36 connexions concordent, avec 12 par phase. Les quatre ergots MR30 restent sans net.
- Contrôle géométrique du PCB importé avec KiCad 9.0.2. Zéro court-circuit signalé et zéro pad à relier. Dix avertissements concernent la recherche des bibliothèques Altium dans la table de bibliothèques de KiCad.
- Six polygones remplis, un par phase sur chaque face, et 54 vias de liaison. La distance minimale entre les contours des grands polygones de phases différentes est de 2,34 mm. Les pistes vers les MR30 suivent un chemin distinct des trois liaisons principales.
- Le STEP de la carte nue s’ouvre en un solide de 90 × 60 × 1,6 mm. Le montage est proposé sur deux faces. Les volumes des connecteurs, des fiches, des leviers et du support restent à vérifier.

## Décisions avant fabrication ou essai

1. Préciser si 80 A nominal et 100 A crête désignent le courant batterie ou le courant de phase, sa valeur efficace, la durée de pointe et le cycle des essais.
2. Choisir un connecteur et un renfort dont le calibre convient au chemin fort courant. La fiche WAGO 2716-103 donne 76 A selon l’IEC et 55 A pour les groupes UL B/C. Confirmer aussi le calibre de chaque paire de bullets et la limite du chemin MR30 complet. Les pistes, vias et fenêtres de masque de cette version ne constituent pas une qualification 80–100 A.
3. Dimensionner le renfort de chaque phase, ses raccords aux connecteurs et ses points de soudure. Les ouvertures de masque centrales sont provisoires. Confirmer le cuivre fini de 70 µm par face, la métallisation et la tenue thermique du trajet complet.
4. Mesurer la partie à souder des bullets réels avant de retenir le perçage actuel de 7,2 mm. Vérifier les plans constructeur et l’accouplement réel des MR30PW-M et MR30PW-FB. Confirmer le repérage des phases en regardant la face d’accouplement de chaque connecteur.
5. Confirmer l’accès sous le circuit au WAGO J1 et au MR30 J7, le dégagement du câblage et les points de fixation du banc. Les trous de montage M3 restent à positionner avec le châssis.
6. Ouvrir le projet dans Altium 26, valider le schéma, contrôler les changements proposés lors de la synchronisation vers le PCB, recalculer les polygones et lancer le DRC natif avant de générer les fichiers de fabrication.

Fiche constructeur du bornier WAGO 2716-103 : https://www.wago.com/2716-103
