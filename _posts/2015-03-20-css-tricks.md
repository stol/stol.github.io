---
layout: post
title: Astuces CSS (article en cours de rédaction)
---


### Centrage vertical/horizontal facile

<p data-height="351" data-theme-id="0" data-slug-hash="ZYwKrd" data-default-tab="result" data-user="stolalex" class='codepen'>See the Pen <a href='http://codepen.io/stolalex/pen/ZYwKrd/'>ZYwKrd</a> by alexishehehe (<a href='http://codepen.io/stolalex'>@stolalex</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Le conteneur de l'élément affiché doit avoir un layout forcé via la propriété `position: relative`, `position: absolute` ou `position: fixed`.

L'élément à centrer doit être mis en `display: absolute` pour être librement positionné.

Les propriétés `top: 50%` et `left: 50%` positionnent le bouton à 50% des hauteurs et largeurs totale du conteneur.

Les propriétés `transform: translateY(-50%)` et `transform: translateX(-50%)` déplace l'élément à -50% de ses propres hauteurs et largeurs.

```css
.container{
  height: 300px;
  position: relative;
}

.vertical-centered{
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

.horizontal-centered{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
```

### Ratios

### box-sizing

### checkbox trick
