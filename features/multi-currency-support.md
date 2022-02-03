# Multi Currency Support

{% hint style="info" %}
Please note that this article only applies to [Shopify Multi-currency](https://help.shopify.com/en/manual/payments/shopify-payments/multi-currency).
{% endhint %}

There are various 3rd party multi-currency providers for Shopify but we are currently only able to support Shopify multi-currency.

You can determine if a store is using Shopify multi-currency by checking if multiple currencies are enabled in the [Shopify Payments ](https://help.shopify.com/en/manual/payments/shopify-payments/multi-currency/setup)section of the store's settings.

If you do not have access to the store's settings, you can navigate to the store and [open Chrome's developer tools](https://developers.google.com/web/tools/chrome-devtools#open). If you switch currencies, entering `Shopify.currency` in the console should reflect the currency change. If it doesn't, the chances are a 3rd party is being used.

## Theme Changes

You will first need to add the snippet below to the nosto-tagging.liquid file in your theme. You can read about how to edit your theme’s code in [Shopify’s help center](https://help.shopify.com/en/manual/using-themes/change-the-layout/theme-code).

Please note that recommendations will not be visible after editing the theme until you complete the multi-currency setup.

The snippet:

```
<div data-gb-custom-block data-tag="if" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1' data-14='1' data-15='1'>

  

<div data-gb-custom-block data-tag="for">

    

<div data-gb-custom-block data-tag="if">

      <div class="nosto_variation" style="display: none;">{{ currency.iso_code }}</div>
    

</div>

  

</div>

</div>
```

![Editing theme file in Shopify](https://user-images.githubusercontent.com/22770093/70220546-6f934880-174f-11ea-812d-94356e47ae36.png)

## Multi-Currency Settings

Once your theme has been updated, you can then go to your multi-currency settings in Nosto.

### Enabling Multi-currency

First set the primary currency to your store’s base currency. Then enable exchange rates and save.

![Setting the primary currency](https://user-images.githubusercontent.com/22770093/70220552-7457fc80-174f-11ea-93ae-908140bf5952.png)

After you save, you will see a warning that no exchange rates have been sent yet. This warning will disappear after a few minutes when we have determined the exchange rates used by your store.

### Product Reindex

At this point, we will need to reindex all of your products. Instructions on how to do this can be found in our [Product Reindex](https://help.nosto.com/en/articles/617677-tools-product-reindex-update) help center article.

### Rounding Rules

We use the default rounding values as [described by Shopify](https://help.shopify.com/en/manual/payments/shopify-payments/multi-currency/conversions). You can change these if you have configured Shopify to use different rounding rules.

![Editing the rounding rules](https://user-images.githubusercontent.com/22770093/70220567-7a4ddd80-174f-11ea-8406-36dbc3923b9a.png)

### Price Formats

Price formatting can be configured at the bottom of the page.

![Price formats](https://user-images.githubusercontent.com/22770093/70220575-7cb03780-174f-11ea-83cb-cd21e14e19eb.png)

![Price format](https://user-images.githubusercontent.com/22770093/70220589-80dc5500-174f-11ea-8864-1caef3e7613b.png)

Finally, you can verify that currency conversion is working as expected by viewing recommendations on your store and comparing the prices of the recommended products with the prices displayed on the product page.

## Fetch Product Prices in Presentment Currency (legacy)

This older approach can be used to fetch the correct product prices for onsite recommendations when Shopify multi-currency features are used which are not supported by Nosto.

Our client script has the function `Nosto.setPresentmentPricesUrl()` which has the following parameters:

* `priceElement` The CSS selector used to find elements where the presentment price should be inserted
* `productUrl` The attribute name of the element that contains the product URL
* `fetchListPrice`Fetch the list price (aka compare\_at\_price). Otherwise fetch the price.
* `variantId`  (Optional) The attribute name of the element that contains the variant id.

This recommendation template snippet works with the function:

```
#foreach($product in $products)
  <span class="nosto_money" x-nosto-url="$!product.url" x-variant-id="123456"></span>
#end
<script type="text/javascript">
  Nosto.setPresentmentPricesUrl('.nosto_money', 'x-nosto-url', true, 'x-variant-id')
<script>
```

If all prices are being formatted with the dollar symbol, regardless of the selected currency, you may need to ensure that the `Shopify.money_format` variable is correctly set to include the currency symbol. If not, you can add this snippet to your theme

```
<script>
  var Shopify = Shopify || {};
  Shopify.money_format = {{ shop.money_format | json }};
</script>
```
