Nosto supports multiple currencies by dynamically converting the catalog prices using your store's exchange rates.

![2.3.8](https://img.shields.io/badge/nosto-2.3.8-red.svg)

If your store view allows retailers to purchase in multiple currencies, the extension will automatically begin using the exchange rates. From version 2.10.0 you can also disable multi currency features and Nosto will not try to do any currency conversions for product prices. If your store view allows purchases only in one single currency you can set the multi currency method to be "Single currency" and Nosto will convert all product prices to that currency using the exchange rates saved in Magento.

![Multiple Currencies](https://user-images.githubusercontent.com/327432/31316109-2ab18b2e-ac2f-11e7-9230-576dbec556c5.png)

If your store has multiple store views and each store view use only one currency, you should set the Multi currency method to be "Single currency".

When using multiple currencies exchange rates are automatically synchronized with Nosto and the recommendations use these rates to dynamically convert the catalog prices from the store's base currency to the customer's active currency.

The customer's active currency is denoted by some embedded semantic markup on all pages. The semantic markup resembles the following snippet:

```
<div class="nosto_variation" style="display: none;">EUR</div>
```

## Enabling / Disabling Multi-Currency

As of version 2.10.0 you can disable multi currency from the Nosto extension's settings (Stores > Configuration > Services > Nosto).

![fullscreen_23_04_18_13_44](https://user-images.githubusercontent.com/15191701/39122156-9af96024-46fc-11e8-9109-00e0e262c776.png)

## Updating Exchange Rates

While the extension has fallback mechanisms to update the exchange rates, you must have cron configured. The extension has a built-in cron handler that updates the exchange rates every hour. This interval is fixed and cannot be altered.