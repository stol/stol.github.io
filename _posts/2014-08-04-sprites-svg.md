---
layout: post
title: La technique des sprites SVG
---

### Préambule

L'iconographie d'un site, c'était encore simple il y'a quelques années. Un fichier par icône, ou utilisation de sprites pour les plus férus de performance.

Parfois c'était un peu compliqué car il fallait gérer les icônes au :hover ou :active. Un peu lourd mais ça se faisait sans trop de désagrément en utilisant des outils automatisant certaines tâches.

Puis les écrans retina sont apparus.

Pour avoir des icônes nettes, en conservant un système de spriting, il faut gérer les tailles x2, x3 voir plus, à multiplier par le nombre d'états et le nombre de tailles de chaque icône. Autant dire que ça devient très très vite chronopage et lourd à gérer.

Mais heureusement il y a le SVG. Plusieurs techniques existent, avec des avantages et des inconvénients.

Je vais expliquer l'une d'entre elles, qui allie facilité d'utilisation, customisation, et qui est bien supportée avec un peu de tweak. Il s'agit de la technique du ``<use xlink:href="fichier.svg#id-icone"></use>``

Elle fonctionne nativement sur les chrome, Firefox et safari modernes, et nécessite un ptit peu de javascript pour fonctionner sur IE9+, ainsi que sur les anciennes version mobiles des navigateurs (android 2.1+).

### Mise en place

#### Le fichier SVG

Vous devez déjà disposer d'un fichier SVG contenant votre iconographie.

Par exemple, sur le site sur lequel je travaille, j'utilise le fichier suivant : 

![](https://www.academiedugout.fr/bundles/udgweb/img/sprites-18px.svg)

J'utilise le service [IcoMoon](https://icomoon.io/app/#/select) pour générer le fichier à partir des fichiers individuels, mais vous pouvez utiliser l'outil de votre choix pour parvenir à la même solution.

Il faut à la fin disposer d'un fichier qui contienne du code similaire à ceci :

    <?xml version="1.0" encoding="utf-8"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="630" height="90" viewBox="0 0 630 90">
    <defs>
    <g id="icon-pictures">
    	<path class="path1" d="M1 7h12v8h-12v-8z"></path>
    	<path class="path2" d="M2 5h13v1h-13v-1z"></path>
    	<path class="path3" d="M14 5h1v8h-1v-8z"></path>
    	<path class="path4" d="M4 3h13v1h-13v-1z"></path>
    	<path class="path5" d="M16 3h1v8h-1v-8z"></path>
    </g>
    <g id="icon-play">
    	<path class="path1" d="M5 3l10 6.001-10 5.999z"></path>
    </g>
    <g id="icon-list">
    	<path class="path1" d="M1 1h16v1h-16v-1z"></path>
    	<path class="path2" d="M1 16h16v1h-16v-1z"></path>
    	<path class="path3" d="M2 1v16h-1v-16h1z"></path>
    	<path class="path4" d="M17 1v16h-1v-16h1z"></path>
    	<path class="path5" d="M5 6h8v1h-8v-1z"></path>
    	<path class="path6" d="M5 9h8v1h-8v-1z"></path>
    	<path class="path7" d="M5 12h8v1h-8v-1z"></path>
    </g>
    <g id="icon-hamburger">
    	<path class="path1" d="M0 13h18v2h-18v-2z"></path>
    	<path class="path2" d="M0 8h18v2h-18v-2z"></path>
    	<path class="path3" d="M0 3h18v2h-18v-2z"></path>
    </g>
    </svg>

Ici, j'ai 4 icones déclarée, chacune disposant d'un identifiant unique : icon-pictures, icon-play, icon-list et icon-hamburger.

Mon fichier d'icônes contient des icônes à la même échelle, sur une grille de 18x18 en l'occurence. Il est possible de mixer des icones de différentes tailles, mais ça oblige à jouer avec la viewbox au cas par cas.

Pour afficher une de ces icônes dans la page, il suffit de l'appeler avec le code

    <svg class="icon" viewbox="0 0 18 18"><use xlink:href="chemin/vers/sprite.svg#icon-play"></use></svg>
    
    .icon{
        width: 18px;
        height: 18px;
    }


ce qui donne 

<style>
.icon{
width: 18px;
height: 18px;
}
</style>

<svg class="icon" viewbox="0 0 18 18"><use xlink:href="images/sprites.svg#icon-play"></use></svg>




