In this article, you will learn how to implement multi-variants in Nosto. When the implementation is complete, you will be able to display different products at different prices to different customer groups. 

**Note:** You can only change the pricing and the availabilities using this feature.

**Note:** You cannot use SKUs with this feature at the time of writing.

You will need to implement the multi-variate tagging if you have any such scenarios:

* Your store has different prices for B2B and B2C customers
* Your store has different prices for logged-in and logged-out customers
* Your store has different prices for loyalty customers
* Your store has different prices and availabilities for different locations

Prior to the multi-variate implementation, ensure that the Nosto tagging is correctly in place. Some of the tagging must be slightly amended to support multi-variants.

## Sending the product metadata

The product update API calls must be amended to denote the primary currency code of the product. Typically, most retailers have a primary currency which is the default currency of the inventory.

For example, a retailer who has different prices for normal and loyal customers would have `GENERAL` as the default variation id and `LOYAL` as an extra variation.

Some additional properties named `variation_id` and `variations` must be placed within the product object.

```json
[
  {
    ...
    ...
    "variation_id": "GENERAL",
    "variations": {
      "LOYAL": {
        "price_currency_code": "USD",
        "availability": "InStock",
        "price": 12.00,
        "list_price": 15.67
      }
    },
    ...
    ...
  }
]'
```

### Do child-products (SKUs) support customer groups?

No. You cannot use SKUs with this feature at the time of writing.

### What about the prices in the cart and the order tagging?

The cart and order tagging can be left as-is but the prices must be in the customer's currently active currency. For example, a customer shopping in Swiss Francs (CHF) should have all the cart items tagged in Swiss Francs (CHF). Failure to do so will result in incorrect prices in any triggered emails such as abandoned cart or order followup.

## Specifying the active variation

Once you have amended the product metadata, you must change the session usage to also pass the active variation of the customer.

```js
nostojs(api => {
  api.defaultSession()
    .setVariation("GENERAL")
});
```

For example, on the site of a retailer, who has different prices for normal (GENERAL) and loyal (LOYAL) customers, if the customer is a logged in customer and is a known loyalty customer, the `nosto_variation` element should show `LOYAL`. If the customer logs out or new customer visits, and there is no way to identify him as a loyal customer, the `nosto_variation` element should show `GENERAL`.

## Enabling multi-variants from the admin

Once the product metadata changes have been done, you need to configure and enable it from your admin panel under **Settings** > **Other** > **Multi-Currency**. Toggle the **Use Multiple Currencies** switch on and **Use Exchange Rates** switch off and set the variation ID of the primary currency via the input field and toggle on the exchange rates switch.

![](https://user-images.githubusercontent.com/327432/36842403-419416ae-1d54-11e8-9bea-a979d7896977.png)

**Note:** Ensure that the Variation ID of the primary currency matches the value sent via the `variation_id` element in the product metadata.

**Note:** Multi-variants cannot be used in conjunction with exchange-rates based multi-currency feature. You must keep the **Use Exchange Rates** switch off.

You will also need to configure the price formatting for your primary and secondary currencies.

## Reviewing your changes

Once you enabled multi-variants you can preview the product prices for different groups by navigating to **Tools** > **Products** and choosing a product.

You will see one or more dropdowns that contain the prices and the availability for that group. 

![](https://user-images.githubusercontent.com/327432/36842669-15cb7412-1d55-11e8-8b48-5f769bb4ecd2.png)

When you have reviewed your set-up, you’re all set and ready to go live with our features. Nosto will automatically handle the different customer groups across its feature set.