---
layout: post
title: Organisation d'un projet Sass
---

### Préambule

Lorsqu'on fait l'intégration d'un site web, rien ne reste simple très longtemps.

Au début c'est facile. On attaque une première page en gardant éventuellement à l'esprit les autres pages similaires. On met tout dans un seul fichier, on compile, on ajoute des règles. Peu à peu les lignes s'additionnent. En utilisant les possibilités offertes par Sass, on commence a créer quelques mixins, quelques variables, quelques héritages. Puis on met de plus en plus de temps à retrouver le code de tel ou tel élément.

Et pour peu qu'on soit plusieurs sur le même fichier, on se télescope, surtout si on n'utilise pas de méthode et qu'on a pas de versionning.

Assez rapidement on externalise quelques infos : ``_variables.scss`` ou ``_mixins.scss``. Puis d'autres, tels que ``_colors.scss``, ``_layout.scss``, ``_menu`` ou que sais-je encore.

Et un beau jour, alors que le site n'est peut-être même pas encore sorti, c'est le bordel. Les fichiers sont interdépendants, ils ont tous besoin les uns des autres. Des règles en écrasent d'autres, des effets de bord arrivent en pagaille, et on fini par ajouter à la fin du fichier principal des règles complexes écrasant des sélecteurs précédents.

Et je ne vous parle pas de devoir exporter toute ou partie du site dans des widgets externes, ou de devoir mettre certains blocs/pages/parties du site en marque blanche, ou de la gestion du responsive.

Oui c'est de l'expérience vécue, et je ne pense pas être le seul.

Pour ne plus jamais reproduire cette façon de fonctionner, 3 choses m'ont permis de passer outre :

- Une organisation de fichier efficace
- Une methodologie robuste (BEM en l'occurence, mais ça fera l'objet d'un post ultérieur)
- L'utilisation des fonctionnalités avancées (notamment l'héritage) de Sass et de certain plugins. (qui fera aussi l'object d'un post ultérieur :p )


### Guide


#### Pré-requis

Installer ``compass`` et le plugin ``compass-import-once`` :

{% highlight bash %}
$ gem install compass compass-import-once
{% endhighlight %}
    

[Compass](http://compass-style.org/) est un framework basé sur Sass, offrant beaucoup de mixins et d'options de configuration.

[Import Once](https://github.com/Compass/compass/tree/master/import-once) est un plugin changeant le comportement de ``@import``, empêchant les imports en double. Cela aura un grand effet sur notre organisation.

Créez ou initialisez votre projet compass via ``$ compass create`` ou ``compass init``, éditez ``config.rb`` selon vos souhaits pour configurer les différents répertoires css/js/img de votre site.

Puis passons à la structure des fichiers elle-même.

#### Structure des fichiers

Je travaille habituellement avec la hiérarchie suivante :

    .
    + _charte.scss
    + _icons.scss
    + _utils.scss
    + _vars.scss
    + app.scss
    + modules/
    |   + _site.scss
    |   + _module-1.html
    |   + _module-2.html
    |   + _module-n.html


``_charte.scss`` contient le look & feel du site : les différents types de textes (couleurs, tailles), les couleurs, les boutons. Mais attention ce fichier ne contient **aucun** code CSS. Uniquement des mixins (``@mixin``) et des classes (``%ma-classe``). Donc si on incluait ce fichier sans utiliser une seule classe ou mixin, aucun code css ne serait généré. Ce fichier devrait faire l'object d'un post ultérieur.

``_icons.scss`` contient la gestion des icônes du site. J'utilise avec des sprites SVG (post ultérieur :)). Il contient des infos du type :

    .mon-icone{
        width: 32px;
        height: 32px;
        fill: red;
        &:hover{
            fill: blue;
        }
    }

``_utils.scss`` contient des mixins, functions, helpers et utils divers et variés. C'est un peu le framework maison. Ce fichier ne génère pas de code en lui-même, mais aide à en générer. Il contient des mixins de media queries, des calculs, etc.

``_vars`` sert pour la configuration : utiliser telle ou telle fonctionnalité, largeurs du site. Pas de couleur ou autres choses ici.

``app.scss`` est le fichier central du site. Il inclu les modules, et rien qu'eux. Ex : 

    @charset "UTF-8";

    @import "modules/site",
            "modules/module-1",
            "modules/module-1",
            "modules/module-n",

Enfin, les ``modules/``. Ces fichiers sont intéressants à plus d'un titre. Chaque fichier contient soit :

* Les règles CSS d'un composant front complexe. Ex : la barre de menu
* Les règles CSS d'un type de données. Ex : sur un site de cuisine, tout ce qui concerne l'affichage des recettes

Ma règle est généralement de créer un fichier par composant que lorsque ce composant devient trop complexe.

#### Création d'un module

Un module est un bout de code dont **l'utilité est complètement délimitée et connue**. Ex : les blocs actualités d'un site de news, un ou plusieurs éléments de formulaire, la landing page qui ne reprend pas les code graphiques du site. Bref, un module ne fait qu'une chose, et n'agit pas sur les autres modules.

Un module peut posséder des **dépendances**.

Un module **génère** du code CSS, à l'inverse d'une dépendance.

Exemple sans utiliser BEM, histoire de rester simple :


    @import "compass/css3/transform",
            "compass/css3/animation",
            "../vars",
            "../charte",
            "../utils";

    $use-css3: false !default; // Est surchargé par la déclaration faite dans _vars.scss

    .mon-module{
    
        border: 1px solid #dadada;
        opacity: 1;
        
        
        @include clearfix; // défini dans _utils.scss
        &.transparent{
            opacity: 0;
        }
    
        @if $use-css3 {
            @include transition(opacity 0.3s ease-out);
        }
    
        .titre{
            @extend titre-simple; // défini dans _charte.scss
        }
        
        .texte{
            @extend texte-moyen; // définit dans _charte.scss
        }
    }

Pour utiliser le module sur le site, il suffit de l'importer dans ``app.scss``.

Etant donné que plusieurs modules peuvent avoir les même dépendances, l'utilisation du plugin "Import Once" est indispensable. Sans lui, chaque ``@import`` provoquerait l'import du fichier correspondant et ça ne marcherait pas.

Nous retrouvons ici un concept similaire à l'injection de dépendances utilisé dans d'autre domaines.

Mais ça permet aussi de pouvoir facilement exporter ce module dans un autre fichier, sans générer de code CSS supplémentaire.

Ex : nous pourrions avoir besoin de d'afficher notre bloc _"mon-module"_ sur un site tierce, dans une iframe. Il serait possible d'appeler la feuille de style principale, mais à quel prix ? Ici, il est possible de faire :

    // export-mon-module.scss
    
    @import "modules/_mon-module.scss";

Le code généré ne comportera **que** le code CSS correspondant au module. Il ne comportera ni les règles globales, ni le grid systeme, ni plein de trucs inutiles.

### Conclusion

Ce système d'organisation est le meilleur que j'ai utilisé jusqu'à présent. Il réponds à tous mes besoins en restant simple tout en offrant suffisamment d'abstraction pour garder le code organisé et propre.

J'ai toutefois quelques améliorations en tête si d'aventure j'en avais besoin un jour.
