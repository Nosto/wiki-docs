# Dynamically specify products to display in widget

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/dynamically-specify-products-to-display-in-widget.md) here.
{% endhint %}

## Overview

Nosto's UGC Social Commerce product is a very powerful resource to an eCommerce site's developer's toolkit. Not only can Nosto's UGC provide direct linking between social media content and a product (or even SKU), those same relationships can be leveraged by the eCommerce CMS or page to specify the content that should be surfaced. By modifying the embed code to specify the products, categories or brands, it is a great complement to the Filter associated with a Widget.

This guide will explore how to filter the widget to be displayed in a Product, Category or Brand pages by only showing content related to those pages.

## Key concepts

### Dynamic Filtering

Dynamic Filtering unlocks special features of Nosto's UGC, outlined in this guide. You will need to have this feature enabled to complete this guide.

### Social Commerce

The Social Commerce product unlocks special features of Nosto's UGC to create Product Tags and other behaviour outline in this guide.

### Product Tags

Like other Tag types in Nosto's UGC, they can be attached to a Tile, in either a 1-to-1 or 1-to-many fashion.

### Widget

Widgets that are embedded for Stacks with Dynamic Filtering enabled have the ability to have their filters augmented with specific Tags.

### Filter

Nosto's UGC Filters drive the content being displayed in the Widget. Dynamic Filtering features allow Tags to be specified to overwrite the Tags chosen for the Filter.

### Example Application

In this guide we will not be creating an application, but really just an example snippet being modified by an imaginary eCommerce system. This translates to following the pattern of a view being passed a variable containing data about the products or product groups we are interested in and the rendering of the widget.

We are assuming that the Stack already has content in it that has been tagged with the relevant Product Tags and that each Product Tag has a Category and/or Brand associated to it.

## The Fun Part

### Getting Started

In this example we will need to do the following:

1. Create a Filter
2. Create a Widget
3. Review relevant tags
4. Prepare embed code for Product page
5. Prepare embed code for Category page
6. Prepare embed code for Brand page

### Create a Filter

Creating a Filter in Nosto's UGC is super easy and outlined in [this Knowledgebase article](http://help.nosto.com/en/articles/5793303-how-to-create-a-filter). Assuming that we would like to only showcase product images, select the following options:

* Name it "Products"
* Set it to sort by "Latest"
* We only want pictures, so we set the Image Type to "Image"

The rest of the options can be left to default.

### Create a Widget

We will be using one Widget that will be customised via the eCommerce system's view. For this reason we just need one Widget and will augment its embed code to specify the product, category or brand to be used.

Create a Waterfall Widget and set its filter to "Products". Save and observe the preview with your content.

### Review relevant Tags

To demonstrate the use of Product Tags, we assume that there are already 2 product tags that we will use for tagging with the following details:

1. **Tag of type:** Product, **ID:** 101, **Name:** "Blue T-Shirt", **URL:** "http://example.com/product/blue-t-shirt", **External Product ID:** "13456743", **Category:** \["T-Shirt"], **Brand:** Nike
2. **Tag of type:** Product, **ID:** 102, **Name:** "Black T-Shirt", **URL:** "http://example.com/product/black-t-shirt", **External Product ID:** "17456941", **Category:** \["T-Shirt"], **Brand:** Patagonia

To view these details, enter the Admin Portal > Engage > Commerce.

We are also assuming that content has been previously tagged with them.

### Prepare embed code for Product Page

There are many different ways in which eCommerce systems generate product pages and product listings. We are assuming a very simple CMS pattern that has one generic view for a product page in this case for the product "Blue T-Shirt"

When deploying the embed code for a product page, products can be specified in two ways:

1. To use their Visual UGC IDs: 101, 102 and 103; or
2. To use their internal (external to Visual UGC) ID, prefixed with "ext:" which are: "ext:13456743", "ext:17456941"

In the embed code the products are specified by adding a `data-tags` parameter to the code and outputting the products as a comma separated list.

```html
<div id="stackla-widget">
    <!-- BEGIN: Your widget code -->
    <div class='stacklafw' data-tags='ext:13456743' data-id='1234' data-hash='aabbccddeeff' data-ct='' data-ttl="30" ></div>
    <script type='text/javascript'>
        (async () => {
            const widget = await import('https://widget-ui.stackla.com/core.esm.js');
            widget.init();
        })();
    </script>
    <!-- END: Your widget code -->
</div>
```

Specifying the data string above as `ext:13456743` would result in the same output if the products were specified using the Visual UGC product tag id `101.`Visual UGC will interpret the `ext:` protocol as specifying the External Product ID and anything else as a Visual UGC Tag ID.

**Note about specifying Tags:**

You may specify up to 10 Tags and the Tags that you specify will overwrite any specified for the Filter. Further you can specify a combination of Product and Content Tags.

### Prepare embed code for Category Page

There are many different ways in which eCommerce systems generate category pages. We are assuming a very simple CMS pattern that has one generic view for a category page in this case for the category "T-Shirt"

When deploying the embed code for a category page, you will need to specify the category as saved in the product tag

In the embed code the categories are specified by adding a `data-categories` parameter to the code and outputting the categories as a comma separated list.

```html
<div id="stackla-widget">
<!-- BEGIN: Your widget code -->
<div class="stackla-widget" **_data-categories='T-Shirt'_** data-id='1234' data-hash='aabbccddeeff' data-ct='' data-ttl="30" ></div>
    <script type="module">
        (async () => {
            const widget = await import('https://widget-ui.stackla.com/core.esm.js');
            widget.init();
        })();
    </script>
<!-- END: Your widget code -->
</div>
```

Using this dynamic filter will result in the widget only showing content that was tagged with products that match the "T-Shirt" category.

**Note about specifying Categories:**

You may specify up to 10 Categories.

### Prepare embed code for Brand Page

There are many different ways in which eCommerce systems generate brand pages. We are assuming a very simple CMS pattern that has one generic view for a category page in this case for the category "Nike"

When deploying the embed code for a brand page, you will need to specify the brand as saved in the product tag

In the embed code the categories are specified by adding a `data-brand` parameter to the code and outputting the categories as a comma separated list.

```html
<div id="stackla-widget">
    <div class="stackla-widget"  data-brand='Nike' data-id='1234' data-hash='aabbccddeeff' data-ct='' data-ttl="30" ></div>
    <script type="module">
        (async () => {
            const widget = await import('https://widget-ui.stackla.com/core.esm.js');
            widget.init();
        })();
    </script>
</div>
```

Using this dynamic filter will result in the widget only showing content that was tagged with products that match the "Nike" brand.

**Note about specifying Brands:**

You may specify up to 10 Brands.
