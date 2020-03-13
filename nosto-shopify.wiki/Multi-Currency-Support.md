In this article, you will learn how to set Nosto up to integrate with [Shopify Multi-currency](https://help.shopify.com/en/manual/payments/shopify-payments/multi-currency).

## Theme Changes
You will first need to add the snippet below to the nosto-tagging.liquid file in your theme. You can read about how to edit your theme’s code in [Shopify’s help center](https://help.shopify.com/en/manual/using-themes/change-the-layout/theme-code).

Please note that recommendations will not be visible after editing the theme until you complete the multi-currency setup.

The snippet:
```
{% if shop.enabled_currencies.size > 1 %}
  {% for currency in shop.enabled_currencies %}
    {% if currency == cart.currency %}
      <div class="nosto_variation" style="display: none;">{{ currency.iso_code }}</div>
    {% endif %}
  {% endfor %}
{% endif %}
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