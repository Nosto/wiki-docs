# How to load external JS and external CSS

{% hint style="warning" %}
You are reading the NextGen Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/loading-external-js-and-css-libraries-to-widgets.md) here.
{% endhint %}

## Overview

This article will demonstrate how to load external JavaScript and CSS into your widgets using our custom code editors.

## Loading the External CSS

Loading the external CSS is very straight-forward. You just need to use the CSS `@import`.

### Example

You can write the following code in your Custom CSS code editor to load [Bootstrap](http://getbootstrap.com) and [Font Awesome](http://fontawesome.io) libraries.

```
@import url(https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.0.0-alpha/css/bootstrap.min.css);
@import url(https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.6.2/css/font-awesome.min.css);
```

[Back to Top](loading-external-js-and-css-libraries-to-widgets.md#top)

## Loading the External JavaScript

In order to load external Javascript files, you can utilise

```js
sdk.addLoadedComponents(["https://google.com/bananas.js"])
```

Congratulations! You have successfully loaded external JS and CSS into your widgets.
