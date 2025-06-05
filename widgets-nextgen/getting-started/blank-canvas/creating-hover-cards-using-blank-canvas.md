# Creating a simple Hover effect using Blank Canvas

* [Overview](creating-hover-cards-using-blank-canvas.md#overview)
* [Sample #1](creating-hover-cards-using-blank-canvas.md#card1)

## Overview

Nosto's UGC Blank Canvas Widget allows users to very quickly and easily design their own Widget templates to suit their organisation's requirements. Included on this page is several simple templates which can be used by customers who want a different way to render image and text content Tiles via Stackla.

Each of the designs have been inspired from [LittleSnippets.net](http://littlesnippets.net), an online blog which contains hundreds of free CSS3/HTML code snippets.

The code and samples below have been optimised for predominately Instagram content, however it can be easily adjusted based upon your requirements.

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #1

In this first sample we are introducing an Angle Line treatment which is present both on the Tile rest state and expands once the user hovers over the respective tile. In this design we introduce the Content Creator and the Tile text on mouse-over.

**Output**

```javascript
<script type="text/javascript">
    (function (d, id) { 
    var t, el = d.scripts[d.scripts.length - 1].previousElementSibling; 
    if (el) el.dataset.initTimestamp = (new Date()).getTime(); 
    if (d.getElementById(id)) 
    return; t = d.createElement('script'); 
    t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js'; 
    t.id = id; (d.getElementsByTagName('head')[0] || 
    d.getElementsByTagName('body')[0]).appendChild(t); 
    }(document, 'stackla-widget-js'));
</script>

```

**Tile Markup**

```
<figure class="tile">
  <img src="{{image}}" alt="{{message}}" />
  <figcaption>
    <h3>@{{user}}</h3>
    <p>{{message}}</p>
  </figcaption>
  <a href="{{original_url}}" target="_blank"></a>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Source+Sans+Pro);
@import url(https://fonts.googleapis.com/css?family=Teko:700);
.tile {
  background-color: #fff;
  color: #ffffff;
  display: inline-block;
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 16px;
  margin: 10px 5px;
  max-width: 400px;
  min-width: 400px;
  max-height: 400px;
  min-height: 400px;
  overflow: hidden;
  position: relative;
  text-align: right;
  width: 100%;
}
.tile *,
.tile *:before,
.tile *:after {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  -webkit-transition: all 0.45s ease;
  transition: all 0.45s ease;
}
.tile img {
  backface-visibility: hidden;
  height: 400px;
  vertical-align: top;
}
.tile:before,
.tile:after {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  content: '';
  background-color: #b81212;
  opacity: 0.5;
  -webkit-transition: all 0.45s ease;
  transition: all 0.45s ease;
}
.tile:before {
  -webkit-transform: skew(30deg) translateX(80%);
  transform: skew(30deg) translateX(80%);
}
.tile:after {
  -webkit-transform: skew(-30deg) translateX(70%);
  transform: skew(-30deg) translateX(70%);
}
.tile figcaption {
  position: absolute;
  top: 0px;
  bottom: 0px;
  left: 0px;
  right: 0px;
  z-index: 1;
  bottom: 0;
  padding: 20px 20px 20px 40%;
}
.tile figcaption:before,
.tile figcaption:after {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #b81212;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.7);
  content: '';
  opacity: 0.5;
  z-index: -1;
}
.tile figcaption:before {
  -webkit-transform: skew(30deg) translateX(100%);
  transform: skew(30deg) translateX(100%);
}
.tile figcaption:after {
  -webkit-transform: skew(-30deg) translateX(90%);
  transform: skew(-30deg) translateX(90%);
}
.tile h3,
.tile p {
  margin: 0;
  opacity: 0;
  letter-spacing: 1px;
}
.tile h3 {
  font-family: 'Teko', sans-serif;
  font-size: 20px;
  font-weight: 700;
  line-height: 1.2em;
  text-transform: uppercase;
}
.tile p {
  font-size: 1.0em;
}
.tile a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
}
.tile:hover h3,
.tile.hover h3,
.tile:hover p,
.tile.hover p {
  -webkit-transform: translateY(0);
  transform: translateY(0);
  opacity: 0.9;
}
.tile:hover:before,
.tile.hover:before {
  -webkit-transform: skew(30deg) translateX(30%);
  transform: skew(30deg) translateX(30%);
  -webkit-transition-delay: 0.05s;
  transition-delay: 0.05s;
}
.tile:hover:after,
.tile.hover:after {
  -webkit-transform: skew(-30deg) translateX(20%);
  transform: skew(-30deg) translateX(20%);
}
.tile:hover figcaption:before,
.tile.hover figcaption:before {
  -webkit-transform: skew(30deg) translateX(50%);
  transform: skew(30deg) translateX(50%);
  -webkit-transition-delay: 0.15s;
  transition-delay: 0.15s;
}
.tile:hover figcaption:after,
.tile.hover figcaption:after {
  -webkit-transform: skew(-30deg) translateX(40%);
  transform: skew(-30deg) translateX(40%);
  -webkit-transition-delay: 0.1s;
  transition-delay: 0.1s;
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)
