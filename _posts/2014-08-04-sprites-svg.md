---
layout: post
title: La technique des sprites SVG (article en cours de rédaction)
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

	</defs>

	</svg>


Ici, j'ai 4 icones déclarée, chacune disposant d'un identifiant unique : icon-pictures, icon-play, icon-list et icon-hamburger.

Mon fichier d'icônes contient des icônes à la même échelle, sur une grille de 18x18 en l'occurence. Il est possible de mixer des icones de différentes tailles, mais ça oblige à jouer avec la viewbox au cas par cas.

Pour afficher une de ces icônes dans la page, il suffit de l'appeler avec le code

    <svg class="icon-18 icon-id-icon" viewbox="0 0 18 18"><use xlink:href="chemin/vers/sprite.svg#id-icon"></use></svg>
    

Et de le faire aller de paire avec du code css
    
    .icon-18{
        width: 18px;
        height: 18px;
    }
    
    .icon-id-icon{
    	/* Personnalisation */
    }

#### Exemples avec l'icône "icon-pictures :"

<style>
.icon{
vertical-align: middle;
display: inline-block;
}
.icon-18{
width: 18px;
height: 18px;
}
.icon-32{
width: 32px;
height: 32px;
}
.icon-64{
width: 64px;
height: 64px;
}
.icon-pictures-variante-1{
fill: orange;
}
.icon-pictures-variante-2{
fill: pink;
}
.icon-pictures-variante-3{
background-color: #fff;
fill: #000;
}
.icon-pictures-variante-3:hover{
background-color: #000;
fill: #fff;
}
</style>


	.icon{
		vertical-align: middle;
		display: inline-block;
	}
	.icon-18{
		width: 18px;
		height: 18px;
	}
	.icon-32{
		width: 32px;
		height: 32px;
	}
	.icon-64{
		width: 64px;
		height: 64px;
	}
	.icon-pictures-variante-1{
		fill: orange;
	}
	.icon-pictures-variante-2{
		fill: pink;
	}
	.icon-pictures-variante-3{
		background-color: #fff;
		fill: #000;
	}
	.icon-pictures-variante-3:hover{
		background-color: #000;
		fill: #fff;
	}

et le code 

	<svg class="icon icon-18 icon-pictures-variante-1" viewbox="0 0 18 18"><use xlink:href="/images/sprites.svg#icon-pictures"></use></svg>
	
	<svg class="icon icon-32 icon-pictures-variante-2" viewbox="0 0 18 18"><use xlink:href="/images/sprites.svg#icon-pictures"></use></svg>
	
	<svg class="icon icon-64 icon-pictures-variante-3" viewbox="0 0 18 18"><use xlink:href="/images/sprites.svg#icon-pictures"></use></svg>


<svg class="icon icon-18 icon-pictures-variante-1" viewbox="0 0 18 18"><use xlink:href="/images/sprites.svg#icon-pictures"></use></svg>

<svg class="icon icon-32 icon-pictures-variante-2" viewbox="0 0 18 18"><use xlink:href="/images/sprites.svg#icon-pictures"></use></svg>

<svg class="icon icon-64 icon-pictures-variante-3" viewbox="0 0 18 18"><use xlink:href="/images/sprites.svg#icon-pictures"></use></svg>


Notez l'effet "mouseover" sur la 3ème image.

Tout est intégralement fait en CSS. Imaginez si vous y ajoutez des transitions ou bien la personalisation fine des path des graphiques directement en CSS.

Avec un seul symbole, j'ai donc créé 3 images de trois tailles et couleurs différentes, dont une avec un effet hover. Le tout parfaitement affiché y compris sur retina.

#### Problème

Ce code ne marche malheureusement pas sur Internet explorer, ni sur les version mobiles anciennes des navigateurs.

#### Solution

Pour y remédier, il est possible de le faire fonctionner sur les navigateurs supportant le SVG inline (directement inclu dans le HTML), à savoir IE9+, safari mobile & android 2.1. On a alors un bon panel de navigateurs supporté.

Le principe est de faire une requète ajax sur le fichier SVG, d'en extraire le symbole correspondant à l'ID demandé, et d'insérer le code trouvé dans le HTML.

[svg4everybody](https://github.com/jonathantneal/svg4everybody) est une toute petite lib se chargeant de le faire pour vous.

Incluez-là après avoir lu le readme, et voilà !

Vous avez maintenant un système d'icône spritées facilement utilisable, personnalisable et performant (1 seule req par page).

Et pour ceux qui veulent du support IE7 ou IE8, il doit être possible de faire du fallback sur du PNG. [Mais bon, en france en juillet 2014, IE8 c'est 1.88%](http://gs.statcounter.com/#desktop+mobile+tablet-browser_version_partially_combined-FR-monthly-201407-201407-bar).

