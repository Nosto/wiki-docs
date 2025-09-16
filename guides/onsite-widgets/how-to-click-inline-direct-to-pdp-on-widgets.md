# How to change click on inline tile behaviour to redirect to PDP in a Widget

{% hint style="warning" %}
You are reading the Classic Widget Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../widgets-nextgen/).
{% endhint %}

## Overview

Nosto's UGC offers the ability to create advanced customizations to your widgets to match your brand needs.

In this guide, we are going explain how you can change the click though behaviour of an inline tile to direct users to PDP.

## Getting Started

#### Create a Widget

Create a new Widget and set the click through behaviour to No clickthrough.

#### For Grid, Masonry, Nightfall, Slider and Quadrant widgets.

* Open the Custom Code editor and click Inline Tile. Under the Javascript section add the following code:

```
$(document).on('widget:ready', function (e, instance) {
    instance
        .on('beforeAppend', function (e, $tile, tileData, $appendRoot) {
            const productTags = tileData.tags_extended.filter(tag => tag.type === 'product' && tag.custom_url);
            if (productTags && productTags.length) {
                $tile[0].onclick = () => window.top.location.href = productTags[0].custom_url;
            }
        });
});
```

* Then click **Save**.

#### For Waterfall and Carousel widgets.

* Open the Custom Code editor and click Inline Tile. Under the Javascript section add the following code:

```
$.extend(Callbacks.prototype, {
    onCompleteJsonToHtml: function($tile, tileData) {
        const productTags = tileData.tags_extended.filter(tag => tag.type === 'product' && tag.custom_url);
        if (productTags && productTags.length) {
            $tile[0].onclick = () => window.top.location.href = productTags[0].custom_url;
        }

        return $tile;
    }
});
```

* Then click **Save**.

In the code above, the first product's url of each tile is used to redirect the user to the PDP page. If you want to use a different product url by product name, please change the way of how to get productTags variable by:

```
const productTags = tileData.tags_extended.filter(tag => tag.type === 'product' && tag.tag === '{productName}')
```
