# How to Load External JS and CSS into Widgets

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