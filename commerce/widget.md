# Widget Implementation

* [Overview](widget.md#overview)
* [Embed Code](widget.md#embed)
* [Dynamic Variables](widget.md#variables)
* [Shopify Add To Cart](integrations/shopify-add-to-cart/shopify-add-to-cart.md)

## Overview

All Nosto's UGC Widgets leverage a similar Embed Code structure, with variations in designs and content handled through a series of parameters.

This structure not only makes it easier for our clients to integrate with various Content Management Systems (CMS) platforms but also means the same Widget can be used for multiple executions as well.

Detailed below are the step-by-step ways in which the dynamic display features of Nosto's UGC Widget can be used for Commerce implementations.

## Embed Code

All Nosto's UGC Widgets leverage a common embed code structure which can be broken into two components. The first component (included below) is the DIV container which defines where the UGC Widget should be rendered on a webpage, as well as includes the various parameters that define what Widget and what Content is loaded.

```
<div class="stackla-widget" data-ct="" data-hash="{{widget-hash}}" data-id="{{widget-id}}" data-title="{{widget-title}}" data-ttl="60" style="width: 100%; overflow: hidden;"></div>
```

The second component is the Widget Javascript. This is the part of the Embed code which is consistent for every Widget. For details on how to store this Javascript file in Google Tag Manager, [click here](integrations/google-tag-manager.md#widget).

```html
<script type="text/javascript">
(function (d, id) {
    var t, el = d.scripts[d.scripts.length - 1].previousElementSibling;
    if (el) el.dataset.initTimestamp = (new Date()).getTime();
    if (d.getElementById(id)) return;
    t = d.createElement('script');
    t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js';
    t.id = id;
    (d.getElementsByTagName('head')[0] || d.getElementsByTagName('body')[0]).appendChild(t);
}(document, 'stackla-widget-js'));
</script>
```

For Commerce Implementations, we will be looking at what variables can be changed and the impact this has on the Widget display.

[Return to Top](widget.md#top)

## Dynamic Variables

All UGC Widgets leverage a common embed code structure which can be broken into two components. The first component (included below) is the DIV container which defines where the UGC Widget should be rendered on a webpage, as well as includes the various parameters that define what Widget and what Content is loaded.

* `data-id`: Widget ID
* `data-hash`  Widget Hash
* `data-filter`: Defines the filter/segment of content to show
* `data-tags`: Defines the tag(s) which need to be present for content to show
* `data-tags-grouped-as`: Specifies the grouping logic for the Tags
* `data-tag-group`: Defines Products for a specific locale
* `data-available-products-only`: Defines whether to show Product information for unavailable products
* `data-ct`: Apply a consistent click-through URL to all Tiles
* `data-tile-id` : Defines a specific Tile(s) to display
* `data-search` : Apply keyword search for message and user

The variables `data-id` and `data-hash` allow customers to define what Widget to display. Note: Only one of the variables, `data-id` and `data-hash`, needs to be defined.

The variable `data-filter` allows for customers to define what content to display within a Widget based on a specific `Filter ID`. The IDs for all Filters can be found within the Nosto Admin Portal under Curate > Filters.

The variable `data-tags` allow customers to define what content to display within a Widget based on the tag(s) that has been assigned to the content. Through this variable brands can define whether they wish to show content for a specific Product or a range of Products by simply adding their `UGC Tag ID` or `External Product ID/SKU`. The following guide provides further information about how to [dynamically specify what Products to display](../guides/onsite-widgets/dynamically-specify-products-to-display-in-widget.md).

The variable `data-tags-grouped-as` allows for customers to define the grouping logic for the Tags they are dynamically refining. The supported values for this variable are `AND` and `OR` with the Widget set to `OR` by default.

The variable `data-available-products-only` allows clients to decide whether Product Tag information should be shown for any Products that are currently `out of stock`. The supported values for this variable are `true` and `false` with the Widget set to `false` by default.

The variable `data-tile-id` allows for clients to define specific Tile(s) of content to display within a Widget. The following guide outlines how to [dynamically specify what Tile to display in a Widget](../guides/onsite-widgets/dynamically-specify-tile-to-display-in-widget.md).

## Localize Widgets

The variable `data-tag-group` allows for clients to define what Locale information should be displayed for a particular Widget. The possible values for this variable are defined as part of the [Product Feed setup](product-feeds.md#locales) and only apply to customers who are importing more than one Product Feed.

By default, Nostio's UGC will show the locale information for the first Product Feed imported.

Customers can also use the Custom Code Editor to further localize the widget using JavaScript. An example of how to localize the copy of the Load More button is documented [here](https://github.com/Stackla/docs/blob/master/docs/guides/how-to-localise-the-load-more-button-on-widgets/README.md).

[Return to Top](widget.md#top)
