# Creating a simple Hover effect using Blank Canvas

{% hint style="warning" %}
You are reading the **Classic Widget Documentation**

Classic widgets are a previous widget version for onsite widgets created before September 23rd.

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../../widgets-nextgen/).
{% endhint %}



* [Overview](creating-hover-cards-using-blank-canvas.md#overview)
* [Sample #1](creating-hover-cards-using-blank-canvas.md#card1)
* [Sample #2](creating-hover-cards-using-blank-canvas.md#card2)
* [Sample #3](creating-hover-cards-using-blank-canvas.md#card3)
* [Sample #4](creating-hover-cards-using-blank-canvas.md#card4)
* [Sample #5](creating-hover-cards-using-blank-canvas.md#card5)
* [Sample #6](creating-hover-cards-using-blank-canvas.md#card6)
* [Sample #7](creating-hover-cards-using-blank-canvas.md#card7)
* [Sample #8](creating-hover-cards-using-blank-canvas.md#card8)

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

## Sample #2

In this sample we introduce a basic hover once the user interacts with the respective tile. Here the treatment sees the hover come from the base of the tile and expand vertically, revealing the additional Tile details.

**Output**

**Tile Markup**

```
<figure class="tile"><img src="{{image}}" alt="sample87"/>
  <figcaption>
    <h3>@{{user}}</h3>
	  <h5><center>{{message}}<br><br>{{source}}</center></h5>
  </figcaption><a href="{{original_url}}" target="_blank"></a>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Raleway);
.tile {
  font-family: 'Raleway', sans-serif;
  position: relative;
  display: inline-block;
  overflow: hidden;
  margin: 10px 8px;
  min-width: 230px;
  max-width: 315px;
  max-height: 315px; 
  width: 100%;
  color: #ffffff;
  font-size: 16px;
  text-align: left;
}
.tile * {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  -webkit-transition: all 0.25s ease;
  transition: all 0.25s ease;
}
.tile:before {
  position: absolute;
  top: 10px;
  bottom: 10px;
  left: 10px;
  right: 10px;
  top: 100%;
  content: '';
  background-color: rgba(51, 51, 51, 0.9);
  -webkit-transition: all 0.25s ease;
  transition: all 0.25s ease;
  -webkit-transition-delay: 0.25s;
  transition-delay: 0.25s;
}
.tile img {
  vertical-align: top;
  height: 315px;
  backface-visibility: hidden;
}
.tile figcaption {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  align-items: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.tile h3,
.tile h5 {
  margin: 0;
  opacity: 0;
  letter-spacing: 1px;
}
.tile h3 {
  -webkit-transform: translateY(-100%);
  transform: translateY(-100%);
  text-transform: uppercase;
  font-weight: 400;
  -webkit-transition-delay: 0.05s;
  transition-delay: 0.05s;
  margin-bottom: 5px;
}
.tile h5 {
  font-weight: normal;
  background-color: #ae895d;
  text-transform: uppercase;
  padding: 3px 10px;
  margin: 20px;
  -webkit-transform: translateY(-100%);
  transform: translateY(-100%);
  -webkit-transition-delay: 0s;
  transition-delay: 0s;
}
.tile a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
}
.tile:hover:before,
.tile.hover:before {
  top: 10px;
  -webkit-transition-delay: 0s;
  transition-delay: 0s;
}
.tile:hover h3,
.tile.hover h3,
.tile:hover h5,
.tile.hover h5 {
  -webkit-transform: translateY(0);
  transform: translateY(0);
  opacity: 1;
}
.tile:hover h3,
.tile.hover h3 {
  -webkit-transition-delay: 0.3s;
  transition-delay: 0.3s;
}
.tile:hover h5,
.tile.hover h5 {
  -webkit-transition-delay: 0.2s;
  transition-delay: 0.2s;
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #3

In this sample we are using CSS to create a block transition effect on the hover-over before revealing the Content Creator and Tile message.

**Output**

**Tile Markup**

```
<figure class="tile"><img src="{{image}}" alt="{{message}}"/>
  <figcaption>
    <h2>@{{user}}</h2>
    <p>{{message}}</p>
  </figcaption>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Source+Sans+Pro:300,500,900);
@import url(https://code.ionicframework.com/ionicons/2.0.1/css/ionicons.min.css);
.tile {
  font-family: 'Source Sans Pro', Arial, sans-serif;
  position: relative;
  float: left;
  overflow: hidden;
  margin: 10px 1%;
  min-width: 350px;
  max-width: 350px;
  max-height: 350px;
  min-height: 350px; 
  width: 100%;
  color: #ffffff;
  text-align: center;
  font-size: 16px;
}
.tile * {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  -webkit-transition: all 0.35s ease;
  transition: all 0.35s ease;
}
.tile img {
  opacity: 1;
  height: 350px;
}
.tile:after,
.tile:before,
.tile figcaption:after,
.tile figcaption:before {
  background: #0a0a0a;
  width: 25%;
  position: absolute;
  content: '';
  opacity: 0;
  -webkit-transition: all 0.3s steps(4);
  transition: all 0.3s steps(4);
  z-index: 1;
  bottom: 100%;
  top: 0;
}
.tile:before {
  left: 0;
  -webkit-transition-delay: 0;
  transition-delay: 0;
}
.tile:after {
  left: 25%;
  -webkit-transition-delay: 0.1s;
  transition-delay: 0.1s;
}
.tile figcaption:before {
  left: 50%;
  -webkit-transition-delay: 0.2s;
  transition-delay: 0.2s;
  z-index: -1;
}
.tile figcaption:after {
  left: 75%;
  -webkit-transition-delay: 0.3s;
  transition-delay: 0.3s;
  z-index: -1;
}
.tile figcaption {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 2;
  padding: 30px;
}
.tile h2,
.tile p,
.tile .icons {
  margin: 0;
  width: 100%;
  opacity: 0;
}
.tile h2 {
  font-weight: 900;
  text-transform: uppercase;
}
.tile p {
  font-weight: 300;
}
.tile .icons {
  position: absolute;
  bottom: 30px;
  left: 0;
  width: 100%;
}
.tile i {
  padding: 0px 10px;
  display: inline-block;
  font-size: 24px;
  color: #ffffff;
  text-align: center;
  opacity: 0.8;
}
.tile i:hover {
  opacity: 1;
}
.tile:hover:after,
.tile.hover:after,
.tile:hover:before,
.tile.hover:before,
.tile:hover figcaption:after,
.tile.hover figcaption:after,
.tile:hover figcaption:before,
.tile.hover figcaption:before {
  bottom: 0;
  opacity: 0.8;
}
.tile:hover figcaption h2,
.tile.hover figcaption h2,
.tile:hover figcaption p,
.tile.hover figcaption p,
.tile:hover figcaption .icons,
.tile.hover figcaption .icons {
  -webkit-transition-delay: 0.4s;
  transition-delay: 0.4s;
}
.tile:hover figcaption h2,
.tile.hover figcaption h2,
.tile:hover figcaption .icons,
.tile.hover figcaption .icons {
  opacity: 1;
}
.tile:hover figcaption p,
.tile.hover figcaption p {
  opacity: 0.7;
}
body {
  height: 380px; 
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #4

For this Sample, we are presenting the User Handle and the Network source on the rest view, and then introducing the Tile text on hover with a series of lines.

In this sample we introduce a basic hover once the user interacts with the respective tile. Here the treatment sees the hover come from the base of the tile and expand vertically, revealing the additional Tile details.

**Output**

**Tile Markup**

```
<figure class="tile">
  <img src="{{image}}" alt="{{message}}" />
  <div class="title">
    <div>
      <h2>@{{user}}</h2>
      <h4>{{source}}</h4>
    </div>
  </div>
  <figcaption>
    <p>{{message}}</p>
  </figcaption>
  <a href="{{original_url}}" target="_blank"></a>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Raleway:400,500,700);
@import url(https://code.ionicframework.com/ionicons/2.0.1/css/ionicons.min.css);
figure.tile {
  font-family: 'Raleway', Arial, sans-serif;
  position: relative;
  float: left;
  overflow: hidden;
  margin: 10px 1%;
  min-width: 400px;
  max-width: 400px;
  min-height: 400px;
  max-height: 400px; 
  width: 100%;
  color: #ffffff;
  text-align: center;
  font-size: 16px;
  background-color: #000000;
}
figure.tile *,
figure.tile *:before,
figure.tile *:after {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  -webkit-transition: all 0.55s ease;
  transition: all 0.55s ease;
}
figure.tile img {
  max-width: 100%;
  backface-visibility: hidden;
  vertical-align: top;
  opacity: 0.9;
}
figure.tile .title {
  position: absolute;
  top: 58%;
  left: 25px;
  padding: 5px 10px 10px;
}
figure.tile .title:before,
figure.tile .title:after {
  height: 2px;
  width: 400px;
  position: absolute;
  content: '';
  background-color: #ffffff;
}
figure.tile .title:before {
  top: 0;
  left: 10px;
  -webkit-transform: translateX(100%);
  transform: translateX(100%);
}
figure.tile .title:after {
  bottom: 0;
  right: 10px;
  -webkit-transform: translateX(-100%);
  transform: translateX(-100%);
}
figure.tile .title div:before,
figure.tile .title div:after {
  width: 2px;
  height: 300px;
  position: absolute;
  content: '';
  background-color: #ffffff;
}
figure.tile .title div:before {
  top: 10px;
  right: 0;
  -webkit-transform: translateY(100%);
  transform: translateY(100%);
}
figure.tile .title div:after {
  bottom: 10px;
  left: 0;
  -webkit-transform: translateY(-100%);
  transform: translateY(-100%);
}
figure.tile h2,
figure.tile h4 {
  margin: 0;
  text-transform: uppercase;
}
figure.tile h2 {
  font-weight: 400;
}
figure.tile h4 {
  display: block;
  font-weight: 700;
  background-color: #ffffff;
  padding: 5px 10px;
  color: #000000;
}
figure.tile figcaption {
  position: absolute;
  bottom: 42%;
  left: 25px;
  text-align: left;
  opacity: 0;
  padding: 5px 60px 5px 10px;
  font-size: 0.8em;
  font-weight: 500;
  letter-spacing: 1.5px;
}
figure.tile figcaption p {
  margin: 0;
}
figure.tile a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
figure.tile:hover img,
figure.tile.hover img {
  zoom: 1;
  filter: alpha(opacity=35);
  -webkit-opacity: 0.35;
  opacity: 0.35;
}
figure.tile:hover .title:before,
figure.tile.hover .title:before,
figure.tile:hover .title:after,
figure.tile.hover .title:after,
figure.tile:hover .title div:before,
figure.tile.hover .title div:before,
figure.tile:hover .title div:after,
figure.tile.hover .title div:after {
  -webkit-transform: translate(0, 0);
  transform: translate(0, 0);
}
figure.tile:hover .title:before,
figure.tile.hover .title:before,
figure.tile:hover .title:after,
figure.tile.hover .title:after {
  -webkit-transition-delay: 0.15s;
  transition-delay: 0.15s;
}
figure.tile:hover figcaption,
figure.tile.hover figcaption {
  opacity: 1;
  -webkit-transition-delay: 0.2s;
  transition-delay: 0.2s;
}
body {
  min-height: 430px; 
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #5

Simple hover effect, with a negative treatment.

**Output**

**Tile Markup**

```
<figure class="tile">
  <img src="{{image}}" alt="{{message}}" />
  <figcaption>
	<h3>{{message}}</h3><br><br>
    <h3>@{{user}}</h3>
    <h4>{{source}}</h4>
  </figcaption>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Montserrat:200);
.tile {
  font-family: 'Montserrat', Arial, sans-serif;
  position: relative;
  display: inline-block;
  overflow: hidden;
  margin: 8px;
  min-width: 230px;
  max-width: 315px;
  max-height: 315px; 
  width: 100%;
  color: #fff;
  text-align: left;
  font-size: 16px;
  background: #000;
}
.tile *,
.tile:before,
.tile:after {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  -webkit-transition: all 0.4s ease;
  transition: all 0.4s ease;
}
.tile img {
  max-width: 100%;
  height: 315px;
  backface-visibility: hidden;
  vertical-align: top;
}
.tile:before,
.tile:after {
  position: absolute;
  top: 20px;
  right: 20px;
  content: '';
  background-color: #fff;
  z-index: 1;
  opacity: 0;
}
.tile:before {
  width: 0;
  height: 1px;
}
.tile:after {
  height: 0;
  width: 1px;
}
.tile figcaption {
  position: absolute;
  left: 0;
  bottom: 0;
  padding: 15px 20px;
}
.tile h3,
.tile h4 {
  margin: 0;
  font-size: 1.1em;
  font-weight: normal;
  opacity: 0;
}
.tile h4 {
  font-size: .8em;
  text-transform: uppercase;
}
.tile a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
}
.tile:hover img,
.tile.hover img {
  zoom: 1;
  filter: alpha(opacity=20);
  -webkit-opacity: 0.2;
  opacity: 0.2;
}
.tile:hover:before,
.tile.hover:before,
.tile:hover:after,
.tile.hover:after {
  opacity: 1;
  -webkit-transition-delay: 0.25s;
  transition-delay: 0.25s;
}
.tile:hover:before,
.tile.hover:before {
  width: 40px;
}
.tile:hover:after,
.tile.hover:after {
  height: 40px;
}
.tile:hover h3,
.tile.hover h3,
.tile:hover h4,
.tile.hover h4 {
  opacity: 1;
}
.tile:hover h3,
.tile.hover h3 {
  -webkit-transition-delay: 0.3s;
  transition-delay: 0.3s;
}
.tile:hover h4,
.tile.hover h4 {
  -webkit-transition-delay: 0.35s;
  transition-delay: 0.35s;
}
body {
  text-align: center;
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #6

The hover treatment in this example sees the Image shrink and disappear to the right on hover-over.

**Output**

**Tile Markup**

```
<figure class="tile">
  <img src="{{image}}" alt="{{message}}" />
  <figcaption>
    <h3>@{{user}}</h3>
    <p>{{message}}</p><a href="{{original_url}}" target="_blank" class="read-more">View Post</a>
  </figcaption>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Raleway:400,500,800); 
figure.tile { 
  font-family: 'Raleway', Arial, sans-serif; 
  position: relative; 
  float: left; 
  overflow: hidden; 
  margin: 10px 1%; 
  min-width: 350px; 
  max-width: 350px; 
  max-height: 350px; 
  min-width: 350px; 	
  width: 100%; 
  color: #ffffff; 
  text-align: left; 
  background-color: #07090c; 
  font-size: 16px; 
} 
figure.tile * { 
  -webkit-box-sizing: border-box; 
  box-sizing: border-box; 
  -webkit-transition: all 0.35s ease; 
  transition: all 0.35s ease; 
} 
figure.tile img { 
  height: 350px; 
  -webkit-transition-delay: 0.2s; 
  transition-delay: 0.2s; 
  backface-visibility: hidden; 
} 
figure.tile figcaption { 
  position: absolute; 
  top: 50%; 
  left: 0; 
  width: 100%; 
  -webkit-transform: scale(0.5) translate(0%, -50%); 
  transform: scale(0.5) translate(0%, -50%); 
  -webkit-transform-origin: 50% 0%; 
  -ms-transform-origin: 50% 0%; 
  transform-origin: 50% 0%; 
  z-index: 1; 
  opacity: 0; 
  padding: 0 30px; 
} 
figure.tile h3, 
figure.tile p { 
  line-height: 1.5em; 
} 
figure.tile h3 { 
  margin: 0; 
  font-weight: 800; 
  text-transform: uppercase; 
} 
figure.tile p { 
  font-size: 0.8em; 
  font-weight: 500; 
  margin: 0 0 15px; 
} 
figure.tile .read-more { 
  border: 2px solid #ffffff; 
  padding: 0.5em 1em; 
  font-size: 0.8em; 
  text-decoration: none; 
  color: #ffffff; 
  display: inline-block; 
} 
figure.tile .read-more:hover { 
  background-color: #ffffff; 
  color: #000000; 
} 
figure.tile:hover img, 
figure.tile.hover img { 
  -webkit-animation: tile 0.45s linear forwards; 
  animation: tile 0.45s linear forwards; 
  -webkit-animation-iteration-count: 1; 
  animation-iteration-count: 1; 
} 
figure.tile:hover figcaption, 
figure.tile.hover figcaption { 
  -webkit-transform: scale(1) translate(0, -50%); 
  transform: scale(1) translate(0, -50%); 
  opacity: 1; 
  -webkit-transition-delay: 0.35s; 
  transition-delay: 0.35s; 
} 
@keyframes tile { 
  50% { 
    -webkit-transform: scale(0.8) translateX(0%); 
    transform: scale(0.8) translateX(0%); 
    opacity: 0.5; 
  } 
  100% { 
    -webkit-transform: scale(0.8) translateX(150%); 
    transform: scale(0.8) translateX(150%); 
    opacity: 0.5; 
  } 
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #7

Simple horizontal slide effect on hover-over.

**Output**

**Tile Markup**

```
<figure class="tile">
  <img src="{{image}}" alt="{{message}}" />
  <figcaption>
    <h2>@{{user}}</h2>
    <h3>{{source}}</h3>
    <p>{{message}}</p>
  </figcaption>
  <a href="{{original_url}}" target="_blank"></a>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Slabo+27px);
@import url(https://fonts.googleapis.com/css?family=Lato);
.tile {
  font-family: 'Lato', sans-serif;
  position: relative;
  float: left;
  overflow: hidden;
  margin: 10px 1%;
  min-width: 230px;
  max-width: 315px;
  min-height: 315px;
  max-height: 315px;
  width: 100%;
  color: #ffffff;
  text-align: left;
  font-size: 16px;
  background-color: #1A1A1A;
}
.tile * {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  -webkit-transition: all 0.45s ease;
  transition: all 0.45s ease;
}
.tile img {
  vertical-align: top;
  max-width: 100%;
  backface-visibility: hidden;
}
.tile figcaption {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  padding: 30px;
  background-color: #202123;
  -webkit-transform: translateX(100%);
  transform: translateX(100%);
  -webkit-box-shadow: 0 0 50px rgba(0, 0, 0, 0.5);
  box-shadow: 0 0 50px rgba(0, 0, 0, 0.5);
}
.tile h2,
.tile h3,
.tile p {
  margin: 0;
}
.tile h2,
.tile h3 {
  font-family: 'Slabo 27px', serif;
  line-height: 1.2em;
}
.tile h2 {
  font-size: 1.9em;
  color: #35ADF9;
}
.tile h3 {
  color: #EBEBEB;
  font-size: 1.3em;
  font-weight: normal;
  letter-spacing: 1px;
}
.tile p {
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  font-size: 0.9em;
  margin-top: 12px;
  padding: 12px 0 15px;
  line-height: 1.5em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.tile a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
}
.tile:hover > img,
.tile.hover > img {
  -webkit-transform: translateX(100%);
  transform: translateX(100%);
}
.tile:hover figcaption,
.tile.hover figcaption {
  -webkit-transform: translateX(0%);
  transform: translateX(0%);
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)

## Sample #8

Simple vertical reveal more on the respective tiles.

**Output**

**Tile Markup**

```
<figure class="tile"><img src="{{image}}" alt="{{message}}"/>
  <figcaption>
    <h3>@{{user}}</h3>
    <h5>{{source}}</h5>
    <blockquote>
      <p>{{message}}</p>
    </blockquote>
  </figcaption><a href="{{source_url}}" target="_blank"></a>
</figure>
```

**CSS**

```
@import url(https://fonts.googleapis.com/css?family=Lato);
@import url(https://fonts.googleapis.com/css?family=Oswald);
@import url(https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css);
.tile {
  font-family: 'Lato', Arial, sans-serif;
  position: relative;
  display: inline-block;
  overflow: hidden;
  margin: 8px;
  min-width: 250px;
  max-width: 310px;
  max-height: 310px; 	
  width: 100%;
  background-color: #000000;
  color: #ffffff;
  text-align: left;
  font-size: 16px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.15);
}
.tile * {
  -webkit-transition: all 0.35s;
  transition: all 0.35s;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
.tile img {
  height: 350px; 
  vertical-align: top;
}
.tile figcaption {
  position: absolute;
  height: 75px;
  left: 15px;
  right: 15px;
  bottom: 15px;
  overflow: hidden;
  padding: 15px;
  background-color: rgba(0, 0, 0, 0.75);
}
.tile h3 {
  font-family: 'Oswald';
  text-transform: uppercase;
  font-size: 20px;
  font-weight: 400;
  line-height: 24px;
  margin: 3px 0;
}
.tile h5 {
  font-weight: 400;
  margin: 0;
  text-transform: uppercase;
  color: #bbb;
  letter-spacing: 1px;
}
.tile blockquote {
  padding: 0;
  margin: 0;
  font-style: italic;
  font-size: 1em;
}
.tile a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.tile:hover figcaption,
.tile.hover figcaption {
  height: calc(85%);
}
```

[Back to Top](creating-hover-cards-using-blank-canvas.md#top)
