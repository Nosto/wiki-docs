# Multi Currency \(Exchange Rates\)

Nosto supports multiple currencies by dynamically converting the catalog prices using your store's exchange rates.

![2.8.0](https://img.shields.io/badge/nosto-2.8.0-green.svg)

If your store view allows retailers to purchase in multiple currencies, the extension will automatically begin using the exchange rates.

![Multiple Currencies](https://user-images.githubusercontent.com/327432/32320267-d8398d60-bfc5-11e7-822f-32210b577277.png)

If your store has multiple store views and each store view has a different currency, the multi-currency is not required and you simply need a separate account for each store view.

Exchange rates are be automatically synchronized with Nosto and the recommendations use these rates to dynamically convert the catalog prices from the store's base currency to the customer's active currency.

The customer's active currency is denoted by some embedded semantic markup on all pages. The semantic markup resembles the following snippet:

```text
<div class="nosto_variation" style="display: none;">EUR</div>
```

## Enabling / Disabling Multi-Currency

There are no toggles for multi-currency. If your store has multi-currency, the exchanges rates are automatically used for dynamic currency conversion.

## Updating Exchange Rates

While the extension has fallback mechanisms to update the exchange rates, you must have cron configured. The extension has a built-in cron handler that updates the exchange rates every hour. This interval is fixed and cannot be altered.

