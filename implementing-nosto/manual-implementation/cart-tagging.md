# Adding the Cart Tagging

On every page load, the cart content must be tagged. The cart contents are the 1:1 representation of the user's mini-cart.

The cart information is used by the Nosto to tailor the recommendations, dispatch abandoned cart emails and fire Facebook pixel events for retargeting purposes.

```markup
<div class="nosto_cart" style="display:none">

    <div class="line_item">
        <span class="product_id">Canoe123</span>
        <span class="quantity">1</span>
        <span class="name">Acme Canoe</span>
        <span class="unit_price">999.00</span>
        <span class="price_currency_code">EUR</span>
    </div>

    <div class="line_item">
        <span class="product_id">Canoe245</span>
        <span class="quantity">3</span>
        <span class="name">Acme Large Canoe</span>
        <span class="unit_price">19.00</span>
        <span class="price_currency_code">EUR</span>
    </div>

</div>
```

> **Note:** The product ID of the product tagging, cart tagging and order tagging must match. Failure to do so will lead to a mismatch in both attribution and statistics across the Nosto product.

### Adding support for advanced use cases

Many e-commerce stores utilize SKU:s or "child" products that are sorted under the same "parent" product. To extend the above example with SKU support refer to [this article](../advanced-implementation/extending-tagging-with-skus.md)

In cases where a product might have multiple prices in differing currencies, you can also add support for multi-currency. Refer to [this article](../advanced-implementation/adding-support-for-multi-currency.md)

### Tagging the cart restore link

If the platform itself has support for persistent shopping cart or other technologies that remember the users cart contents you do not need to worry about filling out the cart when a user returns to the site. If your platform generates a restore cart link you can also send that to Nosto by adding it as a new attribute within the parent container "nosto\_cart".

The following piece of code is just a rough example on how a restore cart could look like. The idea of the example is to document how this is tagged to Nosto.

```markup
<div class="restore_link">https://example.com/cart/restore?cart=4D5C3060-1334-4C63-B6FA-D9D342D88B08</div>
```

## Troubleshooting

Once included on all pages, you can review if the site is transmitting data using the Nosto Debug Toolbar. If you can see cart contents being picked up under "Tagging" → "Cart" then the cart details are correctly set up in the source code. You can further verify your session in the Nosto admin by using the live feed under [https://my.nosto.com/admin/$accountID/liveFeed](https://my.nosto.com/admin/$accountID/liveFeed) to see if Nosto correctly picks up product view → product carted events.

![Nosto debug toolbar cart](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-embed-script-cart.png) ![live-feed-product-cart](https://nosto-campaign-assets.s3.amazonaws.com/images/live-feed-cart.png)

