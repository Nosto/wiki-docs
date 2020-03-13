All thank-you and order-confirmation pages _must_ have the conversion tracking markup. 

The conversion metadata is used for sending personalised order-followup emails, personalise the recommendations e.g. order-related, for segmentation insights and conversion statistics.
 
```html
<div class="nosto_page_type" style="display:none">order</div>
<div class="nosto_purchase_order" style="display:none">
    <span class="order_number">1445</span>
 
    <div class="buyer">
        <span class="email">john.doe@example.com</span>
        <span class="first_name">John</span>
        <span class="last_name">Doe</span>
        <span class="marketing_permission">false</span>
    </div>
 
    <span class="payment_provider">checkmo</span>
    <span class="order_status_code">pending</span>

    <div class="purchased_items">
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
</div>
```

**Note:** The product ID of the product tagging, cart tagging and order tagging must match. Failure to do so will lead to a mismatch in both attribution and statistics across the Nosto product.

### Tagging the buyer

You can omit the buyer tagging either partially, or completely if you do not want Nosto to crawl this information. The user details are stored for possible marketing purposes and mainly observed email address is used in this context. Marketing permission is false by default but if this user has explicitly agreed to receive marketing then you can set it to true manually.

### Tagging the currencies

Currencies should always be represented in the ISO-4471 three-letter format. For example, use the code `USD` instead of `$` to represent the United States Dollar.

### Tagging the payment provider

Payment provider is the payment method or provider used by a shopper to pay the purchase.
Omit the payment provider detail if you do not want Nosto to crawl this information.

### Tagging the order status code

Status code will be used to track the order state.  Different payment providers may use different status codes. 

### Adding support for advanced use cases

Many ecommerce stores utilize SKU:s or "child" products that are sorted under the same "parent" product. To extend the above example with SKU support refer to this article: https://github.com/Nosto/docs-nosto-com/wiki/Extending-tagging-with-SKUs

In cases where a product might have multiple prices in differing currencies you can also add support for multi-currency. Refer to this article: https://github.com/Nosto/docs-nosto-com/wiki/Adding-support-for-multi-currency

### Troubleshooting order tagging:

Once included on all pages, you can review if the site is transmitting data using the Nosto Debug Toolbar. If you can see order contents being picked up under "Tagging" → "Order" then the order details are correctly set up in the source code. 

You can further verify your session in the Nosto admin by using the live feed under: https://my.nosto.com/admin/$accountID/liveFeed to see if Nosto correctly picks up product view → product carted → product bought events. You can export all the order history from the store under Settings → Other → Order report under https://my.nosto.com/admin/$account/account/orders/report

![Nosto debug toolbar order](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-debug-toolbar-order.png)
![live-feed-product-order](https://nosto-campaign-assets.s3.amazonaws.com/images/live-feed-order.png)
