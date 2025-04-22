# Creating an Auto-Scrolling Carousel using Blank Canvas

* [Overview](creating-auto-scrolling-carousel-using-blank-canvas.md#overview)
* [Layout](creating-auto-scrolling-carousel-using-blank-canvas.md#layout)
* [Tile](creating-auto-scrolling-carousel-using-blank-canvas.md#tile)
* [CSS](creating-auto-scrolling-carousel-using-blank-canvas.md#css)
* [Javascript](creating-auto-scrolling-carousel-using-blank-canvas.md#javascript)
* [Final Result](creating-auto-scrolling-carousel-using-blank-canvas.md#final)

## Overview

Nosto's UGC Blank Canvas Widget allows users to very quickly and easily design their own Widget templates to suit their organisation's requirements.

In this guide we are going to create a Carousel Widget, which has been optimised for Tablet and Mobile devices and leverages [Swiper by idangero.us](http://idangero.us/swiper) to create not only a smooth scroll, but also some auto-scrolling behaviour.

In the guide we are going to move through each of the Mustache Partials to build the final Widget.

[Back to Top](creating-auto-scrolling-carousel-using-blank-canvas.md#top)

## Layout

First Step is to build out our Layout partial. Here we are going to load the SVG elements for the left and right arrow and then define the container which will hold the Tiles.

```
<section class="some no-touchevents">
    <svg xmlns="http://www.w3.org/2000/svg" style="display: none;" baseProfile="tiny" version="1.2">
        <symbol id="icon-arrow-left" viewBox="0 0 64 64">
            <title>Arrow Left</title>
            <desc>Scroll Left</desc>
            <polygon points="43,12 36,8 20,32 36,56 43,52 30,32 "></polygon>
        </symbol>
        <symbol id="icon-arrow-right" viewBox="0 0 64 64">
            <title>Arrow Right</title>
            <desc>Scroll Right</desc>
            <polygon points="20,12 27,8 43,32 27,56 20,52 33,32 "></polygon>
        </symbol>
    </svg>
    <main>
        <section>
            <div class="container" id="mouseovercapture">
                <div class="swiper">
                    <div class="some__slider-container swiper-container swiper-container-horizontal swiper-container-rtl" dir="rtl">
                        <ul class="swiper-wrapper some__track">
                            {{#tiles}}
                                <li class="swiper-slide" style="width: 275px; margin-left: 15px;">
                                {{>tpl-tile}}
                                </li>
                            {{/tiles}}
                        </ul>
                    </div>
                    <div class="swiper-navigation">
                        <div class="previous">
                            <svg class="icon">
                                <use xlink:href="#icon-arrow-right"></use>
                            </svg>
                        </div>
                        <div class="next">
                            <svg class="icon">
                                <use xlink:href="#icon-arrow-left"></use>
                            </svg>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>
</section>
```

[Back to Top](creating-auto-scrolling-carousel-using-blank-canvas.md#top)

## Tile

Next step is defining our Tile partial. In this Widget, for our Tiles we simply wish to load the relevant Tile image in the background, and then on mouse-over include the Content Creator, the Tile Message and provide a link back to the original Tile on Social Media.

```
<a target="_blank" href="{{original_url}}" class="tile swiper-lazy swiper-lazy-loaded" style="background-image: url('{{image}}');">
	<p><span class="account">@{{user}}</span>{{{message}}}</p>
</a>
```

[Back to Top](creating-auto-scrolling-carousel-using-blank-canvas.md#top)

## CSS

Next step is our styling. A number of the below styling elements are influenced from the [Swiper by idangero.us](http://idangero.us/swiper) documentation and can be customised to suit your design preferences.

```
@import url('https://fonts.googleapis.com/css?family=Open+Sans:400,700');	
body {
	font-family: 'Open Sans', sans-serif;
	height: 250px;
}
.swiper-slide,
    .swiper-wrapper {
        width: 100%;
        height: 100%;
        position: relative
    }
    .swiper .swiper-navigation .next,
    .swiper-container-no-flexbox .swiper-slide {
        float: left
    }
    .swiper-container {
        margin: 0 auto;
        position: relative;
        overflow: hidden;
        z-index: 1
    }
    .swiper-container-vertical>.swiper-wrapper {
        -ms-flex-direction: column;
        -webkit-flex-direction: column;
        flex-direction: column
    }
    .swiper-wrapper {
        z-index: 1;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        transition-property: -webkit-transform;
        transition-property: transform;
        transition-property: transform, -webkit-transform;
        box-sizing: content-box
    }
    .swiper-container-android .swiper-slide,
    .swiper-wrapper {
        -webkit-transform: translate3d(0, 0, 0);
        -ms-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0)
    }
    .swiper-container-multirow>.swiper-wrapper {
        -webkit-box-lines: multiple;
        -moz-box-lines: multiple;
        -ms-flex-wrap: wrap;
        -webkit-flex-wrap: wrap;
        flex-wrap: wrap
    }
    .swiper-container-free-mode>.swiper-wrapper {
        transition-timing-function: ease-out;
        margin: 0 auto
    }
    .swiper-slide {
        -webkit-flex-shrink: 0;
        -ms-flex: 0 0 auto;
        -ms-flex-negative: 0;
        flex-shrink: 0
    }
    .swiper-container-autoheight,
    .swiper-container-autoheight .swiper-slide {
        height: auto
    }
    .swiper-container-autoheight .swiper-wrapper {
        -ms-flex-align: start;
        -webkit-align-items: flex-start;
        -ms-grid-row-align: flex-start;
        align-items: flex-start;
        transition-property: height, -webkit-transform;
        transition-property: transform, height;
        transition-property: transform, height, -webkit-transform
    }
    .swiper-container .swiper-notification {
        position: absolute;
        left: 0;
        top: 0;
        pointer-events: none;
        opacity: 0;
        z-index: -1000
    }
    .swiper-wp8-horizontal {
        -ms-touch-action: pan-y;
        touch-action: pan-y
    }
    .swiper-wp8-vertical {
        -ms-touch-action: pan-x;
        touch-action: pan-x
    }
    .swiper-button-next,
    .swiper-button-prev {
        position: absolute;
        top: 50%;
        width: 27px;
        height: 44px;
        margin-top: -22px;
        z-index: 10;
        cursor: pointer;
        background-size: 27px 44px;
        background-position: center;
        background-repeat: no-repeat
    }
    .swiper-button-next.swiper-button-disabled,
    .swiper-button-prev.swiper-button-disabled {
        opacity: .35;
        cursor: auto;
        pointer-events: none
    }
    .swiper-pagination {
        position: absolute;
        text-align: center;
        transition: .3s;
        -webkit-transform: translate3d(0, 0, 0);
        -ms-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0);
        z-index: 10
    }
    .swiper-pagination.swiper-pagination-hidden {
        opacity: 0
    }    
    .swiper-container-coverflow .swiper-wrapper,
    .swiper-container-flip .swiper-wrapper {
        -ms-perspective: 1200px
    }
    .swiper-container-cube,
    .swiper-container-flip {
        overflow: visible
    }
    .swiper-container-cube .swiper-slide,
    .swiper-container-flip .swiper-slide {
        pointer-events: none;
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
        z-index: 1
    }
    .swiper-container-cube .swiper-slide .swiper-slide,
    .swiper-container-flip .swiper-slide .swiper-slide {
        pointer-events: none
    }
    .swiper-container-cube .swiper-slide-active,
    .swiper-container-cube .swiper-slide-active .swiper-slide-active,
    .swiper-container-flip .swiper-slide-active,
    .swiper-container-flip .swiper-slide-active .swiper-slide-active {
        pointer-events: auto
    }
    .swiper-container-cube .swiper-slide-shadow-bottom,
    .swiper-container-cube .swiper-slide-shadow-left,
    .swiper-container-cube .swiper-slide-shadow-right,
    .swiper-container-cube .swiper-slide-shadow-top,
    .swiper-container-flip .swiper-slide-shadow-bottom,
    .swiper-container-flip .swiper-slide-shadow-left,
    .swiper-container-flip .swiper-slide-shadow-right,
    .swiper-container-flip .swiper-slide-shadow-top {
        z-index: 0;
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden
    }
    .swiper-container-cube .swiper-slide {
        visibility: hidden;
        -webkit-transform-origin: 0 0;
        -ms-transform-origin: 0 0;
        transform-origin: 0 0;
        width: 100%;
        height: 100%
    }
    .swiper-container-cube.swiper-container-rtl .swiper-slide {
        -webkit-transform-origin: 100% 0;
        -ms-transform-origin: 100% 0;
        transform-origin: 100% 0
    }
    .swiper-container-cube .swiper-slide-active,
    .swiper-container-cube .swiper-slide-next,
    .swiper-container-cube .swiper-slide-next+.swiper-slide,
    .swiper-container-cube .swiper-slide-prev {
        pointer-events: auto;
        visibility: visible
    }
    .swiper-container-cube .swiper-cube-shadow {
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
        height: 100%;
        background: #000;
        opacity: .6;
        -webkit-filter: blur(50px);
        filter: blur(50px);
        z-index: 0
    }
    .swiper-container-fade.swiper-container-free-mode .swiper-slide {
        transition-timing-function: ease-out
    }
    .swiper-container-fade .swiper-slide {
        pointer-events: none;
        transition-property: opacity
    }
    .swiper-container-fade .swiper-slide .swiper-slide {
        pointer-events: none
    }
    .swiper-container-fade .swiper-slide-active,
    .swiper-container-fade .swiper-slide-active .swiper-slide-active {
        pointer-events: auto
    }
    .swiper-scrollbar {
        border-radius: 10px;
        position: relative;
        -ms-touch-action: none;
        background: rgba(0, 0, 0, .1)
    }
    .swiper-container-horizontal>.swiper-scrollbar {
        position: absolute;
        left: 1%;
        bottom: 3px;
        z-index: 50;
        height: 5px;
        width: 98%
    }
    .swiper-container-vertical>.swiper-scrollbar {
        position: absolute;
        right: 3px;
        top: 1%;
        z-index: 50;
        width: 5px;
        height: 98%
    }
    .swiper-scrollbar-drag {
        height: 100%;
        width: 100%;
        position: relative;
        background: rgba(0, 0, 0, .5);
        border-radius: 10px;
        left: 0;
        top: 0
    }
    .swiper-scrollbar-cursor-drag {
        cursor: move
    }
    
    @-webkit-keyframes swiper-preloader-spin {
        100% {
            -webkit-transform: rotate(360deg)
        }
    }
    @keyframes swiper-preloader-spin {
        100% {
            -webkit-transform: rotate(360deg);
            transform: rotate(360deg)
        }
    }
    html:after {
        content: "breakpoint-getComputedStyleTest";
        display: none
    }
    & {
        overflow-x: hidden;
        background: #eee;
        color: #000;
        font-family: 'Open Sans', sans-serif;
		font-size: 5px;
        line-height: 1.667em;
        margin: 0;
        padding: 70px 0 100px
    }
    &:after {
        content: "none";
        display: none
    }
    & .js-breakpoint-none {
        position: absolute
    }
    &.whitebg {
        background: #fff
    }
    *,
    :after,
    :before {
        box-sizing: border-box;
        -moz-box-sizing: border-box
    }
    @media (min-width: 480px) {
        &:after {
            content: "w480";
            display: none
        }
        & .js-breakpoint-w480 {
            position: absolute
        }
    }
    @media (min-width: 768px) {
        &:after {
            content: "w768";
            display: none
        }
        & .js-breakpoint-w768 {
            position: absolute
        }
    }
    @media (min-width: 980px) {
        &:after {
            content: "w980";
            display: none
        }
        & .js-breakpoint-w980 {
            position: absolute
        }
    }
    @media (min-width: 1024px) {
        &:after {
            content: "w1024";
            display: none
        }
        & .js-breakpoint-w1024 {
            position: absolute
        }
    }
    .swiper {
        position: relative
    }
    .swiper .swiper-container {
        width: 100%
    }
    .swiper .swiper-container .swiper-wrapper {
        list-style: none;
        padding: 0;
        margin: 0;
        will-change: transform;
        transition-timing-function: cubic-bezier(.25, .46, .45, .94)
    }
    .swiper .swiper-container .swiper-wrapper .swiper-slide {
        width: 240px;
        position: relative;
        transition: -webkit-transform .9s ease-out;
        transition: transform .9s ease-out;
        transition: transform .9s ease-out, -webkit-transform .9s ease-out;
        -webkit-transform: scale(.92);
        -ms-transform: scale(.92);
        transform: scale(.92)
    }
    @media (min-width: 480px) {
        .swiper .swiper-container .swiper-wrapper .swiper-slide {
            -webkit-transform: scale(1);
            -ms-transform: scale(1);
            transform: scale(1)
        }
    }
    .swiper .swiper-container .swiper-wrapper .swiper-slide.swiper-slide-active {
        -webkit-transform: scale(1);
        -ms-transform: scale(1);
        transform: scale(1)
    }
    .swiper .swiper-navigation {
        display: none;
        position: absolute;
        left: -65px;
        right: -65px;
        top: 50%;
        -webkit-transform: translateY(-50%);
        -ms-transform: translateY(-50%);
        transform: translateY(-50%)
    }
    @media (min-width: 480px) {
        .swiper .swiper-navigation {
            display: block
        }
    }
    .swiper .swiper-navigation .next.swiper-button-disabled,
    .swiper .swiper-navigation .previous.swiper-button-disabled,
    .swiper .tile .swiper-lazy-preloader {
        display: none
    }
    .swiper .swiper-navigation .next,
    .swiper .swiper-navigation .previous {
        cursor: pointer;
        font-size: 34px;
        fill: #a5a5a5;
        width: 75px;
        will-change: fill;
        transition: fill .33s ease-out
    }
    .swiper .swiper-navigation .next .icon,
    .swiper .swiper-navigation .previous .icon {
        width: 100%
    }
    .swiper .swiper-navigation .next:hover,
    .swiper .swiper-navigation .previous:hover {
        fill: #999
    }
    .swiper .swiper-navigation .previous {
        float: right
    }
    .swiper .tile.swiper-lazy-loading .swiper-lazy-preloader {
        display: block
    }
    .tile .buttonplay,
    &.no-touchevents .tile p {
        will-change: opacity;
        transition: opacity .25s ease-out
    }
    .tile {
        display: block;
        position: relative;
        width: 100%;
        height: 100%;
        text-align: left;
        color: #000;
        background: #fff;
        overflow: hidden;
        background-size: cover;
        text-decoration: none
    }
    &.no-touchevents .tile:hover p {
        opacity: 1
    }
    &.no-touchevents .tile:hover .buttonplay {
        opacity: 0
    }
    &.no-touchevents .tile p {
        opacity: 1;
        color: #000;
    }
    &.no-touchevents .tile.swiper-lazy p {
        opacity: 0
    }
    &.no-touchevents .tile.swiper-lazy:hover:after {
        color: #b8b9ba
    }
    &.no-touchevents .tile.swiper-lazy:hover p {
        opacity: 1
    }
    .tile:before {
        content: "";
        display: block;
        padding-bottom: 100%
    }
    .tile span,
    .tile svg {
        pointer-events: none
    }
    .tile .buttonplay {
        fill: #fff;
        position: absolute;
        top: 50%;
        left: 50%;
        margin: -40px 0 0 -40px;
        width: 80px;
        height: 80px
    }
    .tile p {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: 0;
        padding: 15px;
        direction: ltr;
        background: rgba(255, 255, 255, .8);
        height: 100%;
        font-size: 18px;
        line-height: 1.33;
        transition: opacity .2s;
        opacity: 0
    }
    .tile p .account,
    .tile p .date {
        display: none
    }
    .swiper-container.small .tile p {
        font-size: 16px
    }
    @media (min-width: 768px) {
        .swiper-container.small .tile p {
            padding: 10px
        }
        .tile p {
            padding: 25px
        }
        .tile p .account {
            display: block;
            font-weight: 700;
            margin: 0 0 10px;
            text-transform: uppercase
        }
        .tile p .date {
            display: block;
            font-size: 15px
        }
    }
    .container {
        max-width: 1275px;
        margin: 0 auto;
        padding: 0 12.5px
    }
    @media (min-width: 480px) {
        .container {
            padding: 0 65px
        }
    }
    .nowrap {
        white-space: nowrap
    }
    .hidden {
        display: none
    }
p {
	font-size: 14px !important;
}
```

[Back to Top](creating-auto-scrolling-carousel-using-blank-canvas.md#top)

## Javascript

Finally is the Javascript. First step is loading the [Swiper by idangero.us](http://idangero.us/swiper) library. For this Widget we have loaded it from [cdnjs](https://cdnjs.com) and then defined our variables such as where to load from, speed of scroll and the mobile breakpoints.

```
Stackla.loadJS(['https://cdnjs.cloudflare.com/ajax/libs/Swiper/3.3.1/js/swiper.min.js']).then(function() {
	Stackla.loadTilesByFilter(function (tiles) {
		Stackla.render({tiles: tiles});
		var swiper = new Swiper ('.some__slider-container', {
        slidesPerView: 4,
        spaceBetween: 15,
        centeredSlides: !1,
        slideToClickedSlide: !1,
        lazyLoading: !0,
        preloadImages: !1,
        nextButton: ".next",
        prevButton: ".previous",
        observer: !0,
        watchSlidesProgress: !0,
        watchSlidesVisibility: !0,
        updateOnImagesReady: !0,
        speed: 400,
        autoplay: 3e3,
        autoplayDisableOnInteraction: !1,
        loop: !0,
        breakpoints: {
            1024: {
                slidesPerView: 3
            },
            768: {
                slidesPerView: 2,
                centeredSlides: !1,
                spaceBetween: 15,
                slideToClickedSlide: !1
            },
            480: {
                slidesPerView: 1.5,
                centeredSlides: !0,
                spaceBetween: 0,
                slideToClickedSlide: !0
            }
        }
    });
	console.log(swiper);
	});
});
```

[Back to Top](creating-auto-scrolling-carousel-using-blank-canvas.md#top)

## Final Result

The final result should look similar to the Widget below with the auto-scrolling Carousel being responsive for Tablet and Mobile outputs as well.

```html
<script type="text/javascript">
    (function (d, id) { var t, el = d.scripts[d.scripts.length - 1].previousElementSibling; 
        if (el) el.dataset.initTimestamp = (new Date()).getTime(); 
        if (d.getElementById(id)) return; t = d.createElement('script'); 
        t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js'; 
        t.id = id; (d.getElementsByTagName('head')[0] 
                || d.getElementsByTagName('body')[0]).appendChild(t); 
    }
    (document, 'stackla-widget-js'));</script>
```
