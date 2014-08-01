---
layout: post
title: Comment organiser les fichiers Sass d'un site web ?
---

## Préambule

Lorsqu'on fait l'intégration d'un site web, des problèmatiques apparaissent au fil du temps.

Au début c'est facile, on attaque une première page, avec éventuellement d'autres pages similaires en tête. On met tout dans un seul fichier, on compile, on ajoute des règles et peu à peu les lignes s'additionnent. En utilisant les possibilités offertes par Sass, on comment à créer quelques mixins, quelques variables, quelques héritages. On met de plus en plus de temps à trouver où se trouve les règle de tel ou tel élément.

Et pour peu qu'on soit plusieurs à travailler sur le fichier, on se télescope, surtout si on n'utilise pas de méthode et qu'on a pas de versionning.

Assez rapidement on externalise quelques infos : ``_variables.scss`` ou ``_mixins.scss``. Puis d'autres, tels que ``_colors.scss``, ``_layout.scss``, ``_menu`` ou que sais-je encore.

Et un beau jour, alors que le site n'est peut-être même pas encore sorti, c'est le bordel. Les fichiers sont interdépendants, ils ont tous besoin les uns des autres. Des règles en écrasent d'autres, des effets de bord arrivent en pagaille, et on fini par ajouter en fin de fichier des sélecteurs complexes écrasant un sélecteur précédent, car le précédent sélecteur est utilisé à plusieurs endroits du site.

Et je ne vous parle pas de devoir exporter toute ou partie du site dans des widgets externes, ou encore de devoir mettre certains blocs/pages/parties du site en marque blanche, ou de la gestion du responsive.

Oui c'est de l'expérience vécue, et je ne pense pas être le seul.

Pour ne plus jamais reproduire cette façon de fonctionner, 3 choses m'ont permis de passer outre :

- Une methodologie robuste (BEM en l'occurence, mais ça fera l'objet d'un post ultérieur)
- Une organisation de fichier efficace
- L'utilisation des fonctionnalités avancées (notamment l'héritage) de Sass et de certain plugins.














Et les demandes changent, car le client ou le patron a changé d'avis.

Lorsqu'on créé la structure des fichiers Sass utilisés pour générer la feuille de styles CSS d'un site, plusieurs possibilités s'offrent à nous. 

Les problèmes qui se posent sont les suivants :

- Comment garder une 

## 1/ Première possibilité : un seul fichier

Cette option peut-être valide pour les (très) petits sites, graphiquement simples. Le fichier fait au pire quelques centaines de lignes, mais on s'y retrouve sans trop de problème.

Le fichier Sass contient les règles css, quelques variables et mixins, et basta.

## 2/ 
