# Understanding Handlebars

Handlebars is utilised as our server side rendering engine to ensure that HTML is rendered before it reaches the user's screen.

You will notice two core files in your stackla-widget-templates repository

**1) widgets/{widget-name}/layout.hbs**

**2) widgets/{widget-name}/tile.hbs**

The `tile.hbs` file serves as a Handlebars template for rendering individual tiles. It allows you to modify how **one** tile looks in the layout

```handlebars
{{#tile class="grid-item"}}
<div class="tile" data-background-image="{{image_thumbnail_url}}">
  <div class="icon-section">
    <div class="top-section">
      {{#each attrs}}
      {{#ifequals this "instagram.reel"}}
      <div class="content-icon icon-reel"></div>
      {{/ifequals}}
      {{/each}}
      {{#if tags_extended.length}}
      <div class="shopping-icon icon-products"></div>
      {{/if}}
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
</div>
{{/tile}}
```

The `layout.hbs` file serves as the main template for rendering the overall layout of your widget. It extends from a parent template and provides additional functionality to customize the layout.

Here's an excerpt from the `layout.hbs` file:

```handlebars
<div class="ugc-tiles">
    {{#each tiles}}
    <div class="tile" data-id="{{id}}">
      {{parent}}
    </div>
    {{/each}}
  </div>
```

There are a few helpers you can utilise in your templating, they are the following:

* \{{#Ifequals\}} - Checks if two values are equal to one another
* \{{#tile\}} - Creates a tile with the recommended classes to operate with a widget&#x20;
* \{{#if\}} - Check if a value is true.
* \{{#tiles\}} - Loop through each tile

To understand how to best utilise the attributes available to you, please have a look at the [Composition of tiles](creating-a-new-widget/architecture-of-widgets/composition-of-tiles.md) information sheet.
