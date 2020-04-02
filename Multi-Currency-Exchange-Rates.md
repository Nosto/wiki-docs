Nosto supports multiple currencies by dynamically converting the catalog prices using your store's exchange rates.

![2.2.0-RC1](https://img.shields.io/badge/nosto-2.2.0-red.svg)

If your shop view allows retailers to purchase in multiple currencies, the extension will use Shopware exchange rates to make the conversion.

![image](https://user-images.githubusercontent.com/2778820/46143469-bbe91b80-c262-11e8-8ed9-fc33d7e1abdc.png)

If your store has multiple store views and each store view has a different currency, the multi-currency is not required and you simply need a separate account for each store view.

Exchange rates are be automatically synchronised with Nosto and the recommendations use these rates to dynamically convert the catalog prices from the store's base currency to the customer's active currency.

The customer's active currency is denoted by some embedded semantic markup on all pages. The semantic markup resembles the following snippet:

```
<div class="nosto_variation" style="display: none;">EUR</div>
```

## Enabling / Disabling Multi-Currency

Multi-currency comes disabled by default. To enable it, head to Shopware's backend, then Configuration -> Basic Settings.

Once in the `Basic Settings` window, uncollapse `Additional Settings` key, then select `Personalization for Shopware`.

Select the shop front you wish to enable Multi-currency by selecting the correct tab, then select `Exchange Rates` from the dropdown menu and hit the `Save` button.
<img width="1349" alt="screenshot 2018-09-27 at 14 20 40" src="https://user-images.githubusercontent.com/2778820/46143286-28174f80-c262-11e8-9811-b4011f0b1dd2.png">

## Updating Exchange Rates

The exchange rates are updated every time you refresh Shopware's backend page.

