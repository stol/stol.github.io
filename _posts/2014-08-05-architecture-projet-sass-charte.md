---
layout: post
title: Des textes chartés dans un projet Sass
---

### Introduction


Ce post est le 2ème d'une série concernant l'organisation d'un projet Sass/Compass. Ce post se concentre sur le fichier ``_charte.scss``, dont le rôle est de définir les styles des textes, et par extension des boutons, en responsive.

- 1er article : [Organisation d'un projet Sass](/architecture-projet-sass)


### Organisation du code


Les styles doivent être groupés par famille se "dégradant" de la même façon (= ayant la même apparence en mobile) : les règles sont groupées par *font*, graisseur, casse et éventuellement couleur.

Exemple avec des titres en "Roboto Condensed" en majuscule, avec 3 niveaux en desktop, 2 niveaux en tablette et 1 en mobile :

{% highlight scss %}


%roboto-condensed-bold-uppercase-black{
    // C'est la base de ce style. Aura toujours ce style en mobile.
    font-family: 'Roboto Condensed', sans-serif;
    font-weight: 700;
    text-transform: uppercase;
    font-size: 16px;
    line-height: 20px;
    
    // Niveau grand
    &__1{
        @extend %roboto-condensed-bold-uppercase-black;
        @include media-query(tablette){
            font-size: 30px;
            line-height: 36px;
        }
        @include media-query(desktop){
            font-size: 40px;
            line-height: 50px;
        }
    }
    // Niveau moyen
    &__2{
        @extend %roboto-condensed-bold-uppercase-black;
        @include media-query(tablette){
            font-size: 24px;
            line-height: 30px;
        }
        @include media-query(desktop){
            font-size: 30px;
            line-height: 36px;
        }
    }
    // Niveau le plus petit
    &__3{
        @extend %roboto-condensed-bold-uppercase-black;
        @include media-query(tablette){
            font-size: 20px;
            line-height: 26px;
        }
        @include media-query(desktop){
            font-size: 24px;
            line-height: 30px;
        }
    }
}
    
{% endhighlight %}

A noter que l'encapsulation n'a pas de rôle en soi. C'est juste une façon plus pratique d'organiser le code, de le "namespacer" visuellement.

Une fois ces classes définies, nous avons à  notre disposition 4 classes : 

- ``%roboto-condensed-uppercase-black``
- ``%roboto-condensed-uppercase-black__1``
- ``%roboto-condensed-uppercase-black__2``
- ``%roboto-condensed-uppercase-black__3``


Elle sont utilisables dans les modules :

{% highlight scss %}
// Le code CSS d'un bloc "article"
.module-bem-article{
    &__title{
        @extend %roboto-condensed-uppercase-black__1;
    }
    // [...]
}

// Le code CSS d'un bloc "utilisateur"
.module-bem-user{
    &__name{
        @extend %roboto-condensed-uppercase-black__2;
    }
    // [...]
}
{% endhighlight %}

Cette organisation est à répéter autant de fois qu'il y a de polices/tailles/casses différentes.


### Que mettre dans _charte.scss ?


Il faut y mettre tout les style de textes utilisés, et leur versions responsives. Exemple : 

{% highlight scss %}
%roboto-condensed-bold-uppercase-black{
    // cf : le code précédent
}

%roboto-condensed-uppercase-grey{
    // du code similaire, avec gestion des tailles par breakpoint
}

%arial-uppercase-orange{
    // du code similaire, avec gestion des tailles par breakpoint
}

%arial-normal{
    // du code similaire, avec gestion des tailles par breakpoint
    // ici, les textes "normaux" genre les articles
}
{% endhighlight %}


Il faut y mettre les styles "globaux" :


{% highlight scss %}
// Liens dans le corps des textes
%link-in-text{
    color: orange;
    text-decoration: underline;
    &:hover{
        color: brown;
    }
}

// Liens dans les accroches des articles
%link-in-headline{
    color: red;
    font-weight: 400;
    text-decoration: none;
    
    &:hover{
        text-decoration: underline;
    }
}

// Liens de type "lire la suite"
%link-read-more{
    color: orange;
    text-decoration: none;
    border-bottom: 1px dotted orange;
}
{% endhighlight %}

Il faut y mettre les boutons :

{% highlight scss %}
// Propriétés communes aux boutons (si pertinent)
@mixin btn{
    // [...]
}

// Code d'un petit bouton
@mixin btn-small{
    // [...]
}

// Code d'un gros bouton
@mixin btn-big{
    // [...]
}
{% endhighlight %}

Utilisables ailleurs comme ceci (j'utilise des ``@mixins`` car Sass ne permet pas de faire des ``@extend`` dans des media-queries):


{% highlight scss %}
// Le petit bouton est toujours petit
.bnt-small{
    @include btn;
    @include btn-small;
}

// Le gros bouton devient petit en mobile
.bnt-big{
    @include btn;
    @include btn-small;
    @include media-query(desktop){ (
        @include btn-big;
    }
}

// Le gros bouton reste gros en mobile
.bnt-big-forced{
    @include btn-big;
}
{% endhighlight %}


### Conclusion


Ce fichier ``_charte.scss`` pourrait finalement s'appeler ``_texts.scss`` :-)

Quoi qu'il en soit, en utilisant ces méthodes, on a une bonne visibilité sur les styles de textes utilisés dans le site, quelques soient les tailles des devices.

Ce fichier est une dépendance pure, ne générant pas de code CSS.

Il suffit d'inclure ce fichier dans un module tierce pour avoir accès à tout les styles définis.


