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

Si vous n'êtes pas directement responsable de la créa du site, vous devrez travailler étroitement avec la direction artistique pour réussir à produire du bon code. La création des textes utilisés sur un site doit être pensée de façon globale au site.

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

La DA doit connaître les contraintes liées au développment d'un site responsive. Que vous fassiez du "mobile first" ou bien de la "graceful degradation". À vous d'évangéliser :-)

Quoiqu'il en soit, si la DA ne l'a pas à l'esprit, vous devrez créer vos styles avec le mobile first à l'esprit. En mobile first, pour chaque style "desktop", vous devrez vous demander "La différence de taille entre ces 2 textes est-elle pertinente ?".

### Organisation du code

Les styles doivent être groupés par famille se "dégradant" de la même façon (= ayant la même apparence en mobile).

Par exemple sur mobile, un titre peut être d'une seule taille pour tous les contenus. Mais sur tablette, pour 2 types de contenus un titre peut avoir 2 tailles différentes. Par exemple : titre d'article 24px et titre de page membre 36px, et idem en desktop. Ca peut être du à la maquette, et ça ne cause pas de problème.

Sur destop/tablette, la maquette peut offrir de bonnes possibilités de mise en page avec divers niveaux de titres et tailles différentes. Mais sur mobile, la linéarité de l'affichage du contenu limite les niveaux de tailles des titres. La charte est plus simple. Ainsi, dans notre exemple, les différences de tailles des titres des pages user et articles n'existent pas en mobile, où un titre c'est 18px (par ex).

Enfin, dans notre code Sass, les règles sont groupées par font, graisseur, casse et éventuellement couleur.

J'utilise ici la fonctionnalité "BEM" (@at-root) de Sass pour grouper le code de façon claire.

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


