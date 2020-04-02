# Extending tagging with SKUs

Nosto supports individual product SKUs under parent products. If you have not set up [Product tagging](../manual-implementation/product-tagging/) you should start there and extend the tagging if needed.

The SKU attributes should be listed on the last row of the `nosto_product` block that you have already implemented on the product pages.

**What is a SKU?**

Many e-commerce stores have a parent product with individual child products. The parent product is usually something along the lines of "Ski Jacket" whereas the SKUs would then be "Ski Jacket, Blue, Small", "Ski Jacket, Red, Medium". If your store uses SKUs you should add the following attributes to extend your product tagging.

```markup
<div class="nosto_product" style="display:none">
  ...
  ...
  ...
  <span class="nosto_sku">
    <span class="id">1</span>
      <span class="name">S-Orange</span>
      <span class="price">1269.00</span>
      <span class="list_price">1299.00</span>
      <span class="url">http://www.example.com/product/CANOE123#/1-size-s/13-color-orange</span>
      <span class="image_url">http://www.example.com/product/images/CANOE123-1.jpg</span>
      <span class="availability">InStock</span>
      <span class="custom_fields">
        <span class="size">S</span>
        <span class="color">Orange</span>
      </span>
  </span>

  <span class="nosto_sku">
    <span class="id">2</span>
      <span class="name">S-Blue</span>
      <span class="price">1269.00</span>
      <span class="list_price">1299.00</span>
      <span class="url">http://www.example.com/product/CANOE123#/1-size-s/14-color-blue</span>
      <span class="image_url">http://www.example.com/product/images/CANOE123-2.jpg</span>
      <span class="availability">InStock</span>
      <span class="custom_fields">
        <span class="size">S</span>
        <span class="color">Blue</span>
      </span>
  </span>

</div>
```

**Note:** The attribute `custom_fields` can contain whatever unique information for individual SKUs that you can &gt;consider helpful. Frequently used attributes would be size, color, material.

**Extending the cart tagging with SKU metadata**

When tagging the cart contents as outlined here, you can also tag information of the actual SKU that was added to cart. Notice the extra `<span class="sku_id">` attribute.

```markup
<div class="nosto_cart" style="display:none">

    <div class="line_item">
        <span class="product_id">Canoe123</span>
        <span class="sku_id">201-1</span>
        <span class="quantity">1</span>
        <span class="name">Acme Canoe</span>
        <span class="unit_price">999.00</span>
        <span class="price_currency_code">EUR</span>
    </div>

    <div class="line_item">
        <span class="product_id">Canoe123</span>
        <span class="sku_id">201-2</span>
        <span class="quantity">1</span>
        <span class="name">Acme Canoe</span>
        <span class="unit_price">999.00</span>
        <span class="price_currency_code">EUR</span>
    </div>

    <div class="line_item">
        <span class="product_id">Canoe245</span>
        <span class="sku_id">101-1</span>
        <span class="quantity">3</span>
        <span class="name">Acme Large Canoe</span>
        <span class="unit_price">19.00</span>
        <span class="price_currency_code">EUR</span>
    </div>

</div>
```

**Extending the order tagging with SKU metadata**

When tagging the order contents as outlined here, you can also tag information of the actual SKU that was added to cart. Notice the extra `<span class="sku_id">` attribute inside each of the `purchased_items`.

```markup
<div class="nosto_purchase_order" style="display:none">
    <span class="order_number">1445</span>

    <div class="buyer">
        <span class="email">john.doe@example.com</span>
        <span class="first_name">John</span>
        <span class="last_name">Doe</span>
    </div>

    <div class="purchased_items">
        <div class="line_item">
            <span class="product_id">Canoe123</span>
            <span class="sku_id">101-1</span>
            <span class="quantity">1</span>
            <span class="name">Acme Canoe</span>
            <span class="unit_price">999.00</span>
            <span class="price_currency_code">EUR</span>
        </div>

        <div class="line_item">
            <span class="product_id">Canoe245</span>
            <span class="sku_id">101-1</span>
            <span class="quantity">3</span>
            <span class="name">Acme Large Canoe</span>
            <span class="unit_price">19.00</span>
            <span class="price_currency_code">EUR</span>
        </div>
    </div>
</div>
```

## Validating

Once included you can review if the SKUs are picked up by using the [Nosto Debug Toolbar](https://help.nosto.com/get-started/guides/how-to-use-the-nosto-debug-toolbar). If you can see individual SKUs being picked up below the original product details then this is correctly set up.

You can further verify that products are being indexed to the catalogue under the Nosto admin by navigating to Tools → Products \([https://my.nosto.com/admin/$accountID/campaigns/products/list](https://my.nosto.com/admin/$accountID/campaigns/products/list)\)

![Sku debug toolbar](https://nosto-campaign-assets.s3.amazonaws.com/images/sku-toolbar.png) ![Sku product catalogue](https://nosto-campaign-assets.s3.amazonaws.com/images/sku-catalogue.png)

## FAQ

### Do I need to view events for when an SKU is viewed?

No, Nosto does no recommend individual SKUs. While this is something on our roadmap, at the moment, you do not need to send any events when an SKU is selected.

For example, assume you had a product page selling a shoe. In this case, the product tagging would always point to the id of the shoe. No events should be dispatched when the customer selects a particular size such as S, M, L.

