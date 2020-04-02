# Default Product Tagging

Basic tagging

```markup
<div class="nosto_page_type" style="display:none">product</div>
<div class="nosto_product" style="display:none"> 
  <span class="product_id">Canoe123</span>
  <span class="name">Acme Canoe</span>
  <span class="url">https://example.com/canoe123</span>
  <span class="image_url">https://image.example.com/canoe1.jpg</span>
  <span class="availability">Instock</span>
  <span class="price">999.50</span>
  <span class="price_currency_code">USD</span>
</div>
```

Nosto also supports multiple optional values which may enrich the usage of the service, but are not required. These span elements should be inserted into the "nosto\_product" parent container.

Tagging attribute extension

```markup
<span class="category">/Mens/Jackets</span>
<span class="category">/Mens/Jackets/Ski Jackets</span>
<span class="brand">Acme</span>
<span class="description">This is a great product!</span>
<span class="google_category">Interior > Towels</span>
<span class="list_price">1299.00</span>
<span class="tag1">sporty</span>
<span class="tag2">new-in</span>
<span class="tag3">limited-offer</span>
<span class="tag3">add-to-cart</span>
<span class="rating_value">3.8</span>
<span class="review_count">36</span>
<span class="alternate_image_url">https://image.example.com/canoe2.jpg</span>
<span class="alternate_image_url">https://image.example.com/canoe3.jpg</span>
<span class="custom_fields">
  <span class="material">Cotton</span>
  <span class="weather">Summer</span>
</span>
```

### Tagging the prices

Prices must always be denoted in a simple numerical form using dot as the decimal separator. For example, `1.234,45` is invalid while `1234.45` is valid.

### Tagging the categories

Categories must always be delimited by a slash. For example, `/Home/Accessories` is a valid category while `Home > Accessories` is not.

### Tagging the currencies

Currencies should always be represented in the ISO-4471 three-letter format. For example, use the code `USD` instead of `$` to represent the United States Dollar.

### Tagging the availability

The availability of a product is represented by `InStock` or `http://schema.org/InStock` for products that are in stock and saleable. For products that are out of stock or you don't want to be recommended, you can use `OutOfStock` or `http://schema.org/OutOfStock`

### Tagging the Google Categories

The category of your item based on the Google product taxonomy. Use the schema provided by Google here \([https://support.google.com/merchants/answer/6324436?hl=en](https://support.google.com/merchants/answer/6324436?hl=en)\)

### Tagging the Rating

The rating of a product must be represented as a number between 0.0 and 5.0. For example, a product cannot be rated 9.1. You must normalize your rating value to fit our specified range.

### Tagging the Tags and/or Custom fields

The three tag fields, `tags1`, `tags2` and `tags3` are simply labels that can be used to annotate tags like `discounted`, `limited collection` or other use cases where you might want to filter your Nosto recommendations by certain product groupings.

Custom fields accept a key:value pair where the `class` of the attribute is the key. Common use cases are `material`, `color` or other similar unique identifiers.

### Fields that are not exposable in tagging

Nosto also supports two attributes that are not crawlable through tagging. This is due to the sensitive nature of the attributes. They are: `supplier_cost` and `inventory level`. To send these two values to Nosto you will need to use the [Products API](../../../../apis/rest/products/updating-products-using-the-products-api.md).

### Adding support for advanced use cases

Many e-commerce stores utilize SKU:s or "child" products that are sorted under the same "parent" product. To extend the above example with SKU support refer to [this article](../../advanced-implementation/extending-tagging-with-skus.md)

In cases where a product might have multiple prices in differing currencies, you can also add support for multi-currency. Refer to [this article](../../advanced-implementation/adding-support-for-multi-currency.md)

## Troubleshooting

Once included on all pages, you can review if the site is transmitting data using the [Nosto Debug Toolbar](https://help.nosto.com/get-started/guides/how-to-use-the-nosto-debug-toolbar). If you can see product attributes being picked up under "Tagging" then the product details are correctly set up. You can further verify that products are being indexed to the catalog under the Nosto admin by navigating to Tools → Products: [https://my.nosto.com/admin/$accountID/campaigns/products/list](https://my.nosto.com/admin/$accountID/campaigns/products/list)

![Nosto debug toolbar / products](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-product-tagging.png)

![Nosto product catalogue](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-product-catalogue.png)

![live-feed-product-view](https://nosto-campaign-assets.s3.amazonaws.com/images/live-feed-view.png)

