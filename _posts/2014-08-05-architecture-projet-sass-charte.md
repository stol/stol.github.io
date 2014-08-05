---
layout: post
title: Gérer la charte graphique d'un projet Sass/Compass : le fichier _charte.scss (Article en cours de rédaction)
---

### Introduction

Ce post est le 2ème d'une série concernant l'organisation d'un projet Sass/Compass pour un site web moderne. Ce post se concentre sur le fichier ``_charte.scss``.

Ce fichier réponds à plusieurs objectifs :

- Il se concentre sur les textes et par extension, les boutons
- Il définit des styles via des classes Sass, et ne génère pas de code CSS. Il peut être inclu par d'autres fichiers, qui eux génèreront du CSS
- Il organise les styles et permet d'avoir une meilleure visibilité des polices, couleurs et tailles utilisées, responsive compris
- Il offre un modèle d'oganisation du code, permettant de s'y retrouver

### Pas à pas

Si vous n'êtes pas directement responsable de la créa du site, vous devrez travailler étroitement avec la direction artistique pour réussir à produire du bon code.

La DA doit connaître les contraintes liées au développment d'un site responsive. Que vous fassiez du "mobile first" ou bien de la "graceful degradation". À vous d'évangéliser :-)

Quoiqu'il en soit, la création des textes utilisés sur un site doit être pensée de façon globale au site.

Il faut éviter les styles spécifiques à une page en particulier. 

Il faut penser les textes "sémantiquement" et adapter le style en fonction. Ca se rapproche des antiques h1, h2, etc, mais c'est plus poussé.

Exemple pour un site d'actualité, avec quelques règles qui pourraient déterminer une charte :

- les titres des articles peuvent être toujours en minuscules (font X)
- les auteurs en majuscule (font Y)
- Les headlines en majuscules (font Y)
- Les liens en soulignés dans les textes
- Les liens "Lire la suite" non soulignés
- Plusieurs niveaux de remontées d'articles
- Un couleur par rubrique

Et le tout, en responsive, avec plusieurs tailles de polices différentes.



