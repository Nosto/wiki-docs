---
description: >-
  If you're on a site where the cart-content are not accessible when the page is
  rendered, you might need to fetch the cart content over AJAX/CORS and send
  that information to Nosto.
---

# Dynamically sending the cart content

The example below illustrates how to fetch the fresh cart-content over CORS, leverage that to render the cart-tagging and then use the API to sent that information to Nosto.

{% hint style="danger" %}
This example uses advances constructs, leverages CORS, browser modules and may not have compatibility on older browsers.
{% endhint %}

{% code title="" %}
```javascript
function lineItemToHtml(item) {
  return `
    <div class="line_item">
      <span class="product_id">${item.product_id}</span>
      <span class="sku_id">${item.sku_id}</span>
      <span class="quantity">${item.quantity}</span>
      <span class="name">${item.name}</span>
      <span class="unit_price">${item.unit_price}</span>
      <span class="price_currency_code">${item.currency}</span>
    </div>
  `
}

function cartToHtml(cart) {
  return `
    <div class="nosto_cart" style="display:none">
      ${cart.items.map((item) => lineItemToHtml(item))}
    </div>
  `
}

/**
 * This example fetches cart content from an external endpoint, uses 
 * that information to render the cart-tagging and then reloads the 
 * recommendations if needed.
 *
 * This snippet is written under the assumption that the data returned
 * by the endpoint resembles the following
 * {
 *   items: [
 *     product_id: "product_id",
 *     sku_id: "sku_id",
 *     quantity: "quantity",
 *     name: "name",
 *     unit_price: "unit_price",
 *     currency: "currency"
 *   ]
 * }
 */
export default function() {
  nostojs(api => {
    fetch("https://www.example.com/cart")
      .then(response => {
        const tagging = cartToHtml(response.json())

        document.querySelectorAll('.nosto_cart')
          .forEach(element => element.remove());
        document.body.append(tagging)

        api.resendCartTagging();
      })
  })
}
```
{% endcode %}

