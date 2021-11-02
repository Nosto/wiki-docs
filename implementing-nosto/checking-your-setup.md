---
description: >-
  Step by step guide for checking that our client script is tracking events on
  your site.
---

# Checking your setup

### Session view in the debug toolbar <a href="viewing-the-session-view-in-the-debug-toolbar" id="viewing-the-session-view-in-the-debug-toolbar"></a>

1. Navigate to your site in [incognito mode](https://support.google.com/chrome/answer/95464).
2. Load the debug toolbar by appending `nostodebug=true` to your store's URL e.g. `https://example.com?nostodebug=true`
3. Click the login link and navigate to the session view.

![Click the login button](<../.gitbook/assets/Screenshot 2021-09-10 at 12.52.43.png>) ![After logging in, the session tab should be visible on the left-hand side](<../.gitbook/assets/Screenshot 2021-09-10 at 12.53.29-20210910-095337.png>)

### View Category

Navigate to a category page and verify that the View Category event is tracked with the following details:

* Target matches the viewed category.

![Viewing the "Bags" category](<../.gitbook/assets/View Category.png>)

If the category doesn't appear in the session, these pages might help

* [Tagging: category](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website/manual-implementation/category-and-brand-tagging)
* [Session API: category](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-tracking-events#upon-viewing-a-collection)

### View Product

Navigate to a product page and verify that the View Product event is tracked with the following details:

* Target matches the viewed product ID.

![Viewing product 784735895612](<../.gitbook/assets/View Product.png>)

If the product doesn't appear in the session, these pages might help:

* [Tagging: product](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website/manual-implementation/product-tagging)
* [JS API: product](https://docs.nosto.com/techdocs/apis/js-apis/common-examples/sending-product-view-events)
* [Session API: product](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-tracking-events#upon-viewing-a-product)

### View a Recommended Product <a href="view-a-recommended-product" id="view-a-recommended-product"></a>

Click on a product recommendation and verify that the View Product event is tracked with the following details:

* Target matches the viewed product ID.
* Ref matches the previously viewed recommendation slot ID.

![Viewing product 789528674364 from recommendation slot productpage-nosto-14-high-upsell](<../.gitbook/assets/View Recommended Product.png>)

If the product id and recommendation slot id don't appear in the session, these pages might help:

* [JS API: add-to-cart buttons in recommendations](https://docs.nosto.com/techdocs/apis/js-apis/common-examples/sending-add-to-cart-events)
* [Session API: handling attribution](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features#handling-attribution)

### Cart

Add the product to cart and verify that the cart’s contents are correct. Ensure that the product details in the cart match the product that was previously viewed.

* Product ID
* Price & Currency
* Quantity

![](../.gitbook/assets/Cart.png)

If the cart doesn't correctly show in the session view, these pages might help:

* [Tagging: cart](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website/manual-implementation/cart-tagging)
* [JS API: cart](https://docs.nosto.com/techdocs/apis/js-apis/common-examples/dynamically-sending-the-cart-content)
* [Session API: cart](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-managing-sessions#setting-the-cart)

### Order

Purchase the product and verify that the Buy Product event is tracked with the following details:

* Ref matches the recommendation slot id
* Target matches the product id

Wait 30 minutes for the session to expire and then verify that the order shows up in the orders page of the Nosto admin UI.

![Buy product 789528674364 from recommendation slot productpage-nosto-14-high-upsell.](../.gitbook/assets/Order.png)

If the order doesn't correctly show in the session view, these pages might help:

* [Tagging: order](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website/manual-implementation/order-tagging)
* [JS API: order](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-tracking-events#upon-placing-an-order)

{% hint style="info" %}
If it’s not possible to perform a test order, the Nosto admin UI's orders page can be used to review that orders link to known products in the catalogue and that some items have click attribution towards the visible recommendations.
{% endhint %}
