# Using Webfonts in Widgets

{% hint style="warning" %}
You are reading the NextGen Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/using-webfonts-in-widgets.md) here.


{% endhint %}

## Overview

This article will instruct you on how to use web fonts from different providers such as [Google](https://www.google.com/fonts), [Typekit](https://typekit.com), [Fonts.com](http://www.fonts.com) or [Fontdeck](http://fontdeck.com).

## Step 1 - Load Fonts

You will need to download the font files before using them. Let's take a look at some different approaches to load fonts onto our web page.

### IDE Users

If you are an IDE user, you can upload your font files to any given server and use the following code to load them:

````ts
// widget.ts
declare const sdk: ISdk
import { fonts } from "./fonts"

loadWidget({
  config: {
    fonts: fonts
  }
})
```;

```fonts.ts
export const fonts = [
  {
    fontFamily: "HelveticaNeueLtPro",
    src: [
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Roman.ttf", format: "truetype" }
    ],
    fontWeight: "normal",
    fontStyle: "normal"
  },
  {
    fontFamily: "HelveticaNeueLtPro",
    src: [
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Md.woff2", format: "woff2" },
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Md.woff", format: "woff" }
    ],
    fontWeight: 500,
    fontStyle: "normal",
    fontDisplay: "swap"
  },
  {
    fontFamily: "HelveticaNeueLtPro",
    src: [
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Lt.woff2", format: "woff2" },
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Lt.woff", format: "woff" }
    ],
    fontWeight: 400,
    fontStyle: "normal",
    fontDisplay: "swap"
  },
  {
    fontFamily: "HelveticaNeueLtPro",
    src: [
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Bd.ttf", format: "truetype" }
    ],
    fontWeight: "700",
    fontStyle: "normal"
  },
  {
    fontFamily: "HelveticaNeueLtPro",
    src: [
      { url: "https://templates.stackla.com/assets/fonts/helvetica/HelveticaNeueLTPro-Bd.ttf", format: "truetype" }
    ],
    fontWeight: "bold",
    fontStyle: "normal"
  }
]
````

### Non-IDE Users

If you are a non-IDE user, you can do the following:

1. Import the font in your head section:

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:400,500,700&display=swap">
```

2. Add the font to the CSS editor:

```css
@font-face {
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  src: local('Roboto'), local('Roboto-Regular'), url(https://fonts.gstatic.com/s/roboto/v20/KFOmCnqEu92Fr1Mu4mxP.woff2) format('woff2');
}
```

3. Apply the font to your widget:

```css
.ugc-widget {
  font-family: 'Roboto', sans-serif;
}
```

4. Save your changes and publish the widget.

Congratulations! You have successfully added a web font to your widget. You can now use this font in your CSS styles to customize the appearance of your widget.
