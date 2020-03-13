In this article, you will learn how to implement multi-variants in Nosto. When the implementation is complete, you will be able to display different products at different prices to different customer groups. 

**Note:** You can only change the pricing and the availabilities using this feature.

**Note:** You cannot use SKUs with this feature at the time of writing.

You will need to implement the multi-variate tagging if you have any such scenarios:

* Your store has different prices for B2B and B2C customers
* Your store has different prices for logged-in and logged-out customers
* Your store has different prices for loyalty customers
* Your store has different prices and availabilities for different locations

Prior to the multi-variate implementation, ensure that the Nosto tagging is correctly in place. Some of the tagging must be slightly amended to support multi-variants.

## Changes to the product tagging

The product page tagging must be amended to denote the primary variation code of the product.

For example, a retailer who has different prices for normal and loyal customers would have `GENERAL` as the default variation id and `LOYAL` as an extra variation.

An additional span tag must be placed within the product page tagging with a class name `variation_id`. The tag is a child element of the `nosto_product` element.

```html
<div class="nosto_product" style="display: none;">
  ...
  ...
  ...
  <!-- Variation ID for the primary currency --> 
  <span class="variation_id">GENRAL</span>
  <!-- Variation block for a secondary currency -->
  <div class="variation">
    <span class="variation_id">LOYAL</span>
    <span class="price_currency_code">EUR</span>
    <span class="price">27.00</span>
    <span class="list_price">45.19</span>
    <span class="availability">InStock</span>
  </div>
  <!-- Variation block for a secondary currency -->
  <div class="variation">
    <span class="variation_id">B2B</span>
    <span class="price_currency_code">GBP</span>
    <span class="price">24.00</span>
    <span class="list_price">41.55</span>
    <span class="availability">OutOfStock</span>
  </div>
</div>
```

> **Note:** The code in the `variation_id` element must remain static, regardless of the current context. For example, if a loyal customer is logged in, the `variation_id` field would still `GENERAL` and not change.

### What about the prices in the cart and the order tagging?

The cart and order tagging can be left as-is but the prices must be in the customer's currently active currency. For example, a customer shopping in Swiss Francs (CHF) should have all the cart items tagged in Swiss Francs (CHF). Failure to do so will result in incorrect prices in any triggered emails such as abandoned cart or order followup.

## Specifying the active variation

Once you have amended the product tagging, an additional DIV element must be added to all the other pages (including the product page itself). The tag should not be encapsulated in the `nosto_product` DIV tag. The information sent in the tag refers to the segment of the customer.

```html
<div class="nosto_variation" style="display: none;">GENERAL</div>
```

For example, on the site of a retailer, who has different prices for normal (GENERAL) and loyal (LOYAL) customers, if the customer is a logged in customer and is a known loyalty customer, the `nosto_variation` element should show `LOYAL`. If the customer logs out or a new customer visits, and there is no way to identify him as a loyal customer, the `nosto_variation` element should show `GENERAL`.

## Enabling multi-variants from the admin

Once the tagging changed have been done and the API implemented, you need to configure and enable it from your admin panel under **Settings** > **Other** > **Multi-Currency**. Toggle the **Use Multiple Currencies** switch on and **Use Exchange Rates** switch off and set the variation ID of the primary currency via the input field and toggle on the exchange rates switch.

![](https://user-images.githubusercontent.com/327432/36842403-419416ae-1d54-11e8-9bea-a979d7896977.png)

> **Note:** Ensure that the Variation ID of the primary currency matches the value sent via the `variation_id` element in the product tagging.

> **Note:** Multi-variants cannot be used in conjunction with exchange-rates based multi-currency feature. You must keep the **Use Exchange Rates** switch off.

You will also need to configure the price formatting for your primary and secondary currencies.

## Reviewing your changes

Once you enabled multi-variants you can preview the product prices for different groups by navigating to **Tools** > **Products** and choosing a product.

You will see one or more dropdowns that contain the prices and the availability for that group. 

![](https://user-images.githubusercontent.com/327432/36842669-15cb7412-1d55-11e8-8b48-5f769bb4ecd2.png)

When you have reviewed your set-up, you’re all set and ready to go live with our features. Nosto will automatically handle the different customer groups across its feature set.