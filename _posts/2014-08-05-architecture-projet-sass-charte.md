---
layout: post
title: Gérer la charte graphique d'un projet Sass/Compass : le fichier _charte.scss (Article en cours de rédaction)
---

### Introduction

Ce post est le 2ème d'une série concernant l'organisation d'un projet Sass/Compass pour un site web moderne. Ce post se concentre sur le fichier ``_charte.scss``.

Ce fichier se concentre sur les **textes** et par extension, les boutons.

### Organisation du code

Les styles doivent être groupés par famille se "dégradant" de la même façon (= ayant la même apparence en mobile) : les règles sont groupées par font, graisseur, casse et éventuellement couleur.

Exemple avec des titres en "Roboto Condensed" en majuscule, avec 3 niveaux en desktop, 2 niveaux en tablette et 1 seul en mobile :

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

A noter que l'encapsulation n'a pas de rôle en soi. C'est juste une façon plus pratique d'organiser le code, de le "namespacer" visuellement.

Une fois ces classes définies, nous avons à  notre disposition 4 classes : 

    %roboto-condensed-uppercase-black
    %roboto-condensed-uppercase-black__1
    %roboto-condensed-uppercase-black__2
    %roboto-condensed-uppercase-black__3

Et nous pourrons les utiliser dans un module tièrce : 

    .module-bem-article{
        &__title{
            @extend %roboto-condensed-uppercase-black__1;
        }
    }
    
    .module-bem-user{
        &__name{
            @extend %roboto-condensed-uppercase-black__2;
        }
    }
    

Cette organisation est à répéter autant de fois qu'il y a de polices/tailles/casses différentes.


### Que mettre dans _charte.scss ?

Il faut y mettre tout les style de textes utilisés, et leur versions responsives. Exemple : 

    %roboto-condensed-bold-uppercase-black{...}
    
    %roboto-condensed-uppercase-grey{...}
    
    %arial-uppercase-orange{...}
    
    %arial-uppercase-orange{...}

    %arial-normal{...}


Il faut y mettre les styles "globaux"


    %link-in-text{
        color: orange;
        text-decoration: underline;
        &:hover{
            color: brown;
        }
    }

    %link-in-headline{
        color: red;
        font-weight: 400;
        text-decoration: none;
        
        &:hover{
            text-decoration: underline;
        }
    }

    %link-read-more{
        color: orange;
        text-decoration: none;
        border-bottom: 1px dotted orange;
    }

Il faut y mettre les boutons :

    @mixin btn{
        // border, background, etc
    }
    
    @mixin btn-small{
        // Code d'un petit bouton
    }
    @mixin btn-big{
        // Code d'un gros bouton
    }


Utilisables ailleurs comme ceci (j'utise des mixins car Sass ne permet pas de faire des @extend dans des media-queries):


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


### Conclusion

Ce fichier _charte.scss pourrait finalement s'appeler _texts.scss :-)

Quoi qu'il en soit, en utilisant ces methodes, on a une bonne visibilité sur les styles de textes utilisés dans le site, quelques soit la taille des devices.

Ce fichier est une dépendance pure, ne générant pas de code CSS.

Il suffit d'include ce fichier dans un module tièrce pour avoir accès à tout les styles définis.


