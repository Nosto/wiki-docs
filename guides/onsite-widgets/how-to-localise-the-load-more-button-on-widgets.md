# How to localize the load more button on widgets

{% hint style="warning" %}
You are reading the Classic Widget Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../widgets-nextgen/).
{% endhint %}

## Pre-Requisites

* If you are not familiar with customizing widgets using CSS & Javascript, we suggest you start [here](https://developer.stackla.com/guides/styling-widget-expanded-tile/) first as some of those details will come in handy.

## Overview

Nosto's UGC offers the ability to create advanced customizations to your widgets to match your brand needs.

In this guide, we are going explain how you can localize the Load More button when you use the same widgets in different websites/regions.

Please note that this customization is currently not supported for Story widgets.

## Getting Started

#### Create a Widget

Create a new  Widget that supports the Load More button and leave all the settings as per default.

#### Localize the Load More button

* Open the Custom Code editor and click Inline Tile. Under the Javascript section add the following code:

```
var searchParams = new URLSearchParams(window.location.search);
var tagGroup = searchParams.get("tag_group");
Strings.load_more = 'Expand';

if (tagGroup && tagGroup === 'AU') {
Strings.load_more = 'Expand More';
}
if (tagGroup && tagGroup === 'FR') {
Strings.load_more = 'Charger Plus';
}
if (tagGroup && tagGroup === 'PT') {
Strings.load_more = 'Continuar';
}
```

* In the code above, update the tagGroup to match the different Product Feed Group Names for your account. You can find those by going to **UGC Settings > Product Feeds**.
* Then click **Save**.

####
