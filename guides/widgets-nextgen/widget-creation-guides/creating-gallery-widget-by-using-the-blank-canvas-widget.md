---
hidden: true
---

# Creating gallery widget by using the blank canvas

* [Overview](creating-gallery-widget-by-using-the-blank-canvas-widget.md#overview)
* [Step 1 - Settings](creating-gallery-widget-by-using-the-blank-canvas-widget.md#step1)
* [Step 2 - Coding for Layout and Tile Templates](creating-gallery-widget-by-using-the-blank-canvas-widget.md#step2)
* [Step 3 - Coding for CSS](creating-gallery-widget-by-using-the-blank-canvas-widget.md#step3)
* [Step 4 - Coding for JavaScript](creating-gallery-widget-by-using-the-blank-canvas-widget.md#step4)
* [Step 5 - Refinement](creating-gallery-widget-by-using-the-blank-canvas-widget.md#step5)
* [Result - The Gallery Widget](creating-gallery-widget-by-using-the-blank-canvas-widget.md#result)

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/blank-canvas/creating-gallery-widget-by-using-the-blank-canvas-widget.md) here.
{% endhint %}

## Overview

If you need a widget that Nosto's UGC hasn't supplied out of the box yet, Blank Canvas allows you to create your widget from scratch. In this guide, we will show you how to create a Gallery widget with Blank Canvas step by step. You can learn more information about the Blank Canvas Widget in the following guide: [Creating Your Own Widget Type by Using the Blank Canvas Widget](creating-your-own-widget-type-by-using-the-blank-canvas-widget.md).

## Step 1 - Settings

![](../../../.gitbook/assets/nextgen/create-new-widget-page.png)

If the widget is enabled in your stack, you will see the extra widget type, Blank Canvas, appear in your Widget creation page.

Just click the Create Widget button to start.

Before you start getting creative with the Blank Canvas you will need to set it up properly to ensure that it all works.

The **Selected Filter** is the most important setting among them as it involved the data that you can fetch from Stackla.

## Step 2 - Coding for Layout and Tile Templates

Now let's start writing some HTML. Blank Canvas introduces the [Mustache](https://mustache.github.io) template engine and the Mustache Partials feature. In the following example, we will use partial template **tpl-tile** to render **tiles** objects. The **tiles** objects must be prepared in the Custom JS Editor - which we will talk about later.

### Sample Code

#### Layout

```
<div class="ugc-tiles grid grid-inline">
    {{#tiles}}
    {{>tpl-tile options=../options.config}}
    {{/tiles}}
</div>
<load-more />
```

#### Tile

The following Tile template will be included by the **\{{>tpl-tile\}}** syntax above. We will use classnames **.g-tile**, **.g-caption**, **.g-photo**, **.g-img** to style with CSS in the next section.

```

{{#tile class="grid-item"}}
  {{#ifAutoPlayVideo media options.tile_options.auto_play_video }}
    <div class="tile">
      {{playVideo this "100%" "100%"}}
    </div>
  {{else}}
    <div class="tile" data-background-image="{{image_thumbnail_url}}">
      <div class="icon-section">
        <div class="top-section">
          {{#each attrs}}
            {{#ifequals this "instagram.reel"}}
              <div class="content-icon icon-reel"></div>
            {{/ifequals}}
          {{/each}}
          {{#ifHasProductTags this}}
            <div class="shopping-icon icon-products"></div>
          {{/ifHasProductTags}}
        </div>
        <div class="center-section">
          {{#ifequals media "video"}}
            <div class="icon-play"></div>
          {{/ifequals}}
        </div>
        <div class="bottom-section">
          <tile-tags tile-id="{{id}}" variant="dark" mode="swiper" context="grid-inline"></tile-tags>
          <div class="network-icon icon-{{source}}"></div>
        </div>

        <shopspot-icon tile-id={{id}} />
      </div>

      <div class="tile-image-wrapper">
        {{#ifAutoPlayVideo media options.tile_options.auto_play_video }}
          {{playVideo this "100%" "100%"}}
        {{else}}
          <shopspot-icon tile-id={{id}}></shopspot-icon>
          {{#lazy image_thumbnail_url }}{{/lazy}}
        {{/ifAutoPlayVideo}}
        <div class="swiper-lazy-preloader"></div>
        <div class="description overlay">
          <div class="caption">
            <div class="caption-paragraph">{{message}}</div>
          </div>
        </div>
      </div>
    </div>
  {{/ifAutoPlayVideo}}
{{/tile}}
```

In the above sample code, we created a tile template that will display the image and caption with the **id**, **emoji**, **message**, and **image** properties. We have provided quite a few Tile properties so that you can assemble your own structure easily. Please check the [Tile Properties section in the API documentation](../../../api-docs/javascript/widgets/blank-canvas.md#helper-methods-tile-properties) for more details.

To preview the result of HTML template, we need to get data using JavaScript. Click on the **fork** button on the JavaScript panel, you will get the minimal code needed.

Click on the button "Update Preview", you should see plain stacked images and captions.

## Step 3 - Coding for CSS

All of the CSS rules reside within the Shadow DOM, so you don't need to worry about any conflicts with your page. Please make use of the **@import** directive to import the external CSS stylesheets you need.

### Sample Code

```

#nosto-ugc-container {
  background-color: var(--widget-background);
  padding: var(--margin);

  .tile {
    background-size: cover;
    background-position: center;
    height: 100%;
    width: 100%;
    position: relative;

    .icon-play {
      top: 43%;
      left: 0;
      right: 0;
      margin: auto;
    }

    .tile-image-wrapper {
      position: relative;
      width: 100%;
      overflow: hidden;
      border-radius: 0;
    }
    
    .tile-image-wrapper img {
        width: 100%;
        display: block;
        transition: transform 0.3s ease;
        border-radius: 0;
    }
    
    .description.overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.7);
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        opacity: 0;
        transition: opacity 0.4s ease;
        z-index: 10;
    }
    
    .caption {
        max-width: 90%;
    }
    
    .caption-paragraph {
        font-size: 14px;
        line-height: 1.4;
        overflow: hidden;
        display: -webkit-box;
        -webkit-line-clamp: 4;
        -webkit-box-orient: vertical;
        text-overflow: ellipsis;
    }
    
    .caption-paragraph a {
        color: #ffcc00;
        text-decoration: none;
        font-weight: bold;
    }

    .tile-image-wrapper:hover .description.overlay {
        opacity: 1;
    }
  }
}

    
```

Click on button "Update Preview", you should see images aligned in rows.

## Result - The Gallery Widget

![](../../../.gitbook/assets/step5-preview.png)

After applying all the Layout, Tile, CSS, and JS code by the above code, you should get a Gallery Widget [(Live demo in JS Bin)](http://output.jsbin.com/zicake/1).

That's it! Get your code and embed your Gallery Widget!!

## Live Demo!

```html
<div id="ugc-widget" data-ttl="300" data-hash="681063c4163ef" data-id="64527" data-hash="681063c4163ef" data-id="64527" data-title="Grid - Gallery Widget Test" data-ttl="300" data-tag-group=""></div>
<script>
    (() => {
        const script = document.createElement('script');
        script.type = 'text/javascript';
        script.src = 'https://widget-ui.teaser.stackla.com/core.js';
        script.onload = (() => {
            // You can customise the widget by passing in parameters to the object below.
            const widget = new window.ugc.widget({});
            widget.init();
        });
        document.body.appendChild(script);
    })();
</script>
```
