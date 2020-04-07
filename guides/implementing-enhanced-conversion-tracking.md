# Implementing enhanced conversion tracking

Nosto tracks all conversions via Shopify's APIs and Webhooks. Unlike implementations on other e-commerce platforms, Nosto for Shopify doesn't require the placement of any conversion tracking scripts. However, if you are using a custom checkout gateway, the conversion tracking might not work as expected. We suggest that you liaise with our support personnel prior to proceeding with this.

In order to add the conversion tracking snippet, we recommend reading up on Shopify's guide to [adding conversion tracking to your thank you page.](https://help.shopify.com/manual/orders/status-tracking/add-conversion-tracking-to-thank-you-page)

In the conversion tracking box, simply paste the following snippet.

```text
<script type="text/javascript" src="//connect.nosto.com/include/shopfiy-xxxxxxx" async></script>
<div class="nosto_page_type" style="display:none">order</div>
<div class="nosto_purchase_order" style="display:none">
  <span class="order_number">{{ order_number }}</span>
  <div class="buyer">
    <span class="email">{{ customer.email }}</span>
    <span class="first_name">{{ customer.first_name }}</span>
    <span class="last_name">{{ customer.last_name }}</span>
  </div>
  <div class="purchased_items">
    {% for line_item in order.line_items %}
    <div class="line_item">
        <span class="product_id">{{ line_item.product.id }}</span>
        <span class="sku_id">{{ line_item.variant_id }}</span>
        <span class="quantity">{{ line_item.quantity }}</span>
        <span class="name">{{ line_item.title }}</span>
        <span class="unit_price">{{ line_item.price | money_without_currency }}</span>
        <span class="price_currency_code">{{ shop.currency }}</span>
    </div>
    {% endfor %}
  </div>
</div>
```

> **Note:** Remember to replace the same account id in the conversion snippet with your own. Your account id always is `shopify-` followed by a sequence of numbers.

