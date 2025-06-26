# Customise Add to Cart Widget Experience

## Overview

Shopify Add To Cart is a feature that allows you to create a seamless shopping experience for your customers by allowing them to add products to their cart directly from your UGC Widgets.

## Developer Guide

This guide should assist you with basic customization of the Shopify Add To Cart functionality. We will continue to update this guide with more customization options as they become available.

## Before you get started...

### Default Add to Cart Widget Experience

Our Widget templates come with a default Shopify Add to Cart experience, which can be used as-is. See the below example of the default experience for each widget template:

**Slider Widget**

<figure><img src="../../../.gitbook/assets/1st Page.png" alt="" width="375"><figcaption></figcaption></figure>

**Quadrant Widget**

<figure><img src="../../../.gitbook/assets/1st Page (1).png" alt="" width="375"><figcaption></figcaption></figure>

**Nightfall Widget**

<figure><img src="../../../.gitbook/assets/1st Page (2).png" alt="" width="375"><figcaption></figcaption></figure>

**Story Widget**

<figure><img src="../../../.gitbook/assets/1st Page (3).png" alt="" width="375"><figcaption></figcaption></figure>

**Waterfall, Direct Uploader, Grid, Masonry and Carousel Widgets**

<figure><img src="../../../.gitbook/assets/1st Page (4).png" alt="" width="375"><figcaption></figcaption></figure>

## Note

When customizing UGC widgets, Javascript & CSS codes are modified within the custom code editor of the widget. The section should be 'Expanded Tile'.

## Enable Add to Cart Feature

For the widget to show the Add to Cart experience make sure you enable the setting by going to:

* Expanded Tile Configuration
* Select the Shopify Add to Cart option
* Click Preview and Save

<figure><img src="../../../.gitbook/assets/Stories Create Widget - Settings - None (1).png" alt=""><figcaption></figcaption></figure>

## Customising CSS Properties of the Add To Cart Functionality

The following elements can be customized using our predefined selectors. This can be added to our UGC code editor.

**.ugc-add-to-cart-button** Utilised to customize the Add To Cart button

**.ugc-add-to-cart-colorpicker-text** Utilised to customize the text of the color picker

**.ugc-add-to-cart-colorpicker-ring** Utilised to customize the container for the ring of the color picker

**.ugc-add-to-cart-colorpicker-ring-after:** Utilized to customize the ring of the color picker

**.ugc-add-to-cart-colorpicker** Utilised to customize the container for the color picker

**.ugc-add-to-cart-colorpicker-inner** Utilised to customize the inner container for the color picker

**.ugc-add-to-cart-other-variant-selector** Utilised to customize the container for the other variant selector

**.ugc-add-to-cart-size picker** Utilised to customize the container for the size picker

**.ugc-add-to-cart-sizepicker-btn** Utilised to customize the button for the size picker

**.ugc-add-to-cart-price** Utilised to customize the price of the product

**.ugc-add-to-cart-container** Utilised to customize the container for the add-to-cart functionality

### Example customization

If you wish to change the border of the variant selector to be a square instead of a circle, you can modify the existing border-radius.

For example:

````
```css
.ugc-add-to-cart-colorpicker-ring:after {
    border-radius: 0;
}

.ugc-add-to-cart-colorpicker-ring {
    border-radius: 0;
}

.ugc-add-to-cart-colorpicker-inner {
    border-radius: 0;
}
```
````

### Changing the structure of the variant selectors

If you wish to move the parent divs around or add custom HTML between the components, you can utilize the following code in the UGC Widget Expanded Tile code editor and modify it as required.

```
    window.ugc.options.shopify = {
        rawTemplate: `
        <div data-component="color-variant-picker"></div>
        <div data-component="size-variant-picker"></div>
        <div data-component="other-variant-picker"></div>
        <div data-component="add-to-cart-button"></div>
        `
    }  
```

For example, I may wish to add a div around the color-variant-picker so I can decorate it a bit differently, I can do this by doing so.

```
    window.ugc.options.shopify = {
        rawTemplate: `
        <div id="my-variant-selector">
            <p>Test!</p>
            <div data-component="color-variant-picker">
            </div>
        </p>
        <div data-component="size-variant-picker"></div>
        <div data-component="other-variant-picker"></div>
        <div data-component="add-to-cart-button"></div>
        `
    }  
```

This will enable the addition of the 'Test!' text between variant selectors, and will also create the my-variant-selector div around the color-variant-picker.

### Customising The Add To Cart Error / Success Messages

If you would like to modify the error / success messages that are displayed when adding a product to the cart, you can do so by adding the following code to the UGC Widget Expanded Tile code editor.

```
    window.ugc.options.shopify = {
        successTitle: ‘Great! ’,
        successMessage: ‘Item has been added to cart.’,
        errorTitle: ‘Uh oh!’,
        errorMessage: ‘Item has not been added to cart.’
    }
```

### Customising The Add To Cart Button Text

If you would like to modify the text associated with the add to cart button, you can do so with the following:

```
    window.ugc.options.shopify = {
        addToCartButtonText: ‘Add To Cart’
    }
```

### Using partial color matches

By default, our color variant picker has a strict matching algorithm. This means that if you have a color variant named 'Black' and a color variant named 'Black Navy', the color variant picker will not match the 'Black' variant. However, if you wish to use partial matching, you can do so by adding the following code to the UGC Widget Expanded Tile code editor.

```
    window.ugc.options.shopify = {
        usePartialColorMapping: true
    }
```

By default, we provide a list of 1000+ colors which we can default to, this can be helpful to match to those colors loosely.

### Customising Color Variant Selection for individual Widgets

Adding custom color swatches to your variant selectors is a great way to make your UGC Widgets stand out and enhance the customer's experience.

By default, the widgets will use what's configured in the Global Mapping settings. However, within the Widgets' Custom Code Editor, you can add the following code to your Javascript section to add custom color swatches to your variant selector for a specific widget.

```javascript
window.ugc.options.shopify = {
  mappings: {
    "colors": [
        {
            "name": "Celeste Olive Mix",
            "displayName": "Olive Mix",
            "value": "https://images.ctfassets.net/09hbx69ra4p2/7DQrNNXRhl02EDxhOmNtlW/830408f2ac9bfa5c44dbe63f07ab53ab/swatch_solids-ankle-8pk-fa23-celesteolivemix.png"
        },
        {
            "name": "Black navy mix",
            "displayName": "Black Navy Mix",
            "value": "#e6e6e6"
        }]
    }
};
```

Note the Celeste Olive Mix Color and how it utilizes an image as the value. This is how you can add custom image icons to your color swatches.

However, if you wish to map a color instead of utilizing a custom image, you can add the hexadecimal value of the color instead, as shown in the black navy mix.

If you wish to change the label of the mapping, you can utilize the "displayName" property. This is how it will be displayed, but will default to the name property value.

### Implementing Add To Cart callback function to update mini cart

How the mini cart is updated depends on the client's set up. On the UGC side, a callback function is available that can be used to update the mini cart after a successful Add To Cart action. You can achieve this by adding the following code to the UGC Widget's Expanded Tile code editor.

```
    window.ugc.options.shopify = {
        onAddToCart: function(selectedVariant) {
            //add custom logic here to update mini carts, e.g., window.sideCart.handleAddToCartEvent() 
        }
    }
```

### Adding Shopify Add To Cart To Blank Canvas Widgets

In-order to load add to cart on a blank canvas widget, you will need to load this JS file in your widget: https://stackla.com/media/js/dist/shopify-add-to-cart.bundle.js

Once this JS file is loaded, you will need to create an element to store their add-to-cart content: i.e.

Once that is complete, you will need to invoke the following method on your code, targeting your newly created element: window.ugc.renderShopifyToDOM(`stacklapopup-add-to-cart-${product.id}`, mapping);

The mapping variable is utilized in the same way the settings above are, you can use this to customize the add-to-cart functionality.
