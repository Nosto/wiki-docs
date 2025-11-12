# Shipping to production

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../../../../onsite-widgets/) here.

**Note: This feature is unique to NextGen widgets**
{% endhint %}

In-order to ship your widget to production, you can utilise the assets built in the dist folder.

<figure><img src="../../../../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

There are a few ways you can utilise these to ensure your widget is production ready:

**1) Self host your CSS/JS assets, and update the templates in the widget editor**

You can utilise a CDN to host your CSS and JS files for the widget, and you can load them via the sdk by injecting the scripts into the placement:&#x20;

Before injecting the scripts, dont forget to add the layout and tile handlebars data to the widget editor.

**CSS**

`@import url('https://example.com/styles.css')`

**JS**

`sdk.addLoadedComponents(['https://example.com/widget.js']);`

This should be performed inside the Javascript box and css box of the widget editor

**2) Update the layouts, and templates in the widget editor**

Place the dist file contents into the corresponding boxes on the widget editor and click Preview Changes.

<figure><img src="../../../../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

