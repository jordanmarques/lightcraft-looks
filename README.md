# lightcraft-looks

Bibliothèque de looks pour [LightCraft](https://github.com/storytold/lightcraft), versionnée.
Un fichier `.lcpreset` par look : c'est du JSON, donc un `git diff` montre quel réglage a changé.

## Les looks

| Fichier | Look | En bref |
|---|---|---|
| `leica-m.lcpreset` | Leica M | Rendu couleur des Leica M numériques récents : noirs denses, hautes lumières douces, verts calmés, bleus profonds. |
| `tri-x-1600-moriyama.lcpreset` | Tri-X 1600 Moriyama | Tri-X 400 poussée de deux stops, façon Daido Moriyama : noir et blanc dur, ombres bouchées, gros grain. |
| `portra-400.lcpreset` | Portra 400 | Négatif couleur doux et légèrement surexposé : contraste faible, noirs relevés, tons chauds et pastel. |
| `cinestill-800t.lcpreset` | CineStill 800T | Pellicule cinéma tungstène : ambiance froide bleu-vert, lumières chaudes, halo sur les hautes lumières. |
| `wong-kar-wai.lcpreset` | Wong Kar-wai | *Chungking Express* / *Les Anges déchus* : dominante verte, couleurs saturées, grain fort, contraste marqué. |

Dans LightCraft, ils sont rangés dans le groupe de presets **Looks**.

## Ce que contient un look

Tous les looks écrivent les mêmes groupes de réglages, y compris à leur valeur neutre, pour qu'un
look remplace entièrement le précédent au lieu de s'y cumuler :

couleur ou noir et blanc, tonalité, courbe, couleur, mélangeur, virage, étalonnage, effets,
vignetage, grain, netteté, masques.

Restent **hors** des looks, car propres à chaque photo : balance des blancs, recadrage, géométrie,
corrections optiques.

Conséquence : appliquer un look écrase les retouches manuelles faites dans ces groupes. Appliquer le
look d'abord, affiner ensuite.

## Limites connues

- Ces looks sont des interprétations réglées à l'œil à partir de descriptions publiées, pas des
  profils mesurés sur les appareils ou les pellicules.
- Ce qui relève de l'optique ou de la prise de vue n'est qu'imité ou absent : micro-contraste et
  bokeh Leica, flou de bougé de Moriyama, flou saccadé de Wong Kar-wai.
- **CineStill 800T** : le halo est approché par un masque qui teinte les hautes lumières ; il ne
  déborde pas autour des sources comme le vrai. Pour le plein effet, refroidir la balance des blancs
  à la main (3200–3400 K de nuit).
- **Wong Kar-wai** : dosé sur une scène de nuit. De jour, baisser l'intensité du preset (50–70 %).
- **Portra 400** : pas encore vérifié sur un portrait.
- Un reset complet d'une photo désactive sa correction d'objectif (coins sombres, lignes courbes
  sur un grand-angle) : la réactiver avant de juger un look.

## Installer les looks

Dans LightCraft : menu *File → Import Presets*, puis choisir les fichiers `.lcpreset` (ou le dossier).

En ligne de commande, sur l'application lancée avec `--control 7980` :

```sh
lightcraft-cli run --connect 127.0.0.1:7980 preset.import 'paths=["/chemin/vers/lightcraft-looks"]' group=Looks
```

## Mettre à jour le dépôt

Le dépôt ne suit pas LightCraft tout seul : après avoir modifié un look dans l'application, il faut
réexporter son fichier puis commiter.

```sh
lightcraft-cli run --connect 127.0.0.1:7980 presets.list          # retrouver l'id du preset
lightcraft-cli run --connect 127.0.0.1:7980 preset.export \
    path="$PWD/leica-m.lcpreset" 'ids=["user.41.leica-m"]'
```

Les ids (`user.41.leica-m`, …) sont ceux de la bibliothèque d'origine ; ils peuvent différer après
un import sur une autre machine.
