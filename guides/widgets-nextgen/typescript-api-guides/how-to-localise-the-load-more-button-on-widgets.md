# How to localise the load more button on widgets

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/how-to-localise-the-load-more-button-on-widgets.md) here.
{% endhint %}

## Pre-Requisites

## Overview

Nosto's UGC offers the ability to create advanced customizations to your widgets to match your brand needs.

In this guide, we are going explain how you can localize the Load More button when you use the same widgets in different websites/regions.

Please note that this customization is currently not supported for Story widgets.

## Getting Started

#### Create a Widget

Create a new Widget that supports the Load More button and leave all the settings as per default.

#### Localize the Load More button

* Open the Custom Code editor and click Inline Tile. Under the Javascript section add the following code:

```
var tagGroup = sdk.getTagGroup();

if (tagGroup && tagGroup === 'AU') {
    sdk.querySelector('load-more #buttons #load-more').innerText = 'Load More';
}
if (tagGroup && tagGroup === 'FR') {
    sdk.querySelector('load-more #buttons #load-more').innerText = 'Charger plus';
}
if (tagGroup && tagGroup === 'PT') {
    sdk.querySelector('load-more #buttons #load-more').innerText = 'Carregar mais';
}
```

* In the code above, update the tagGroup to match the different Product Feed Group Names for your account. You can find those by going to **UGC Settings > Product Feeds**.
* Then click **Save**.

####
