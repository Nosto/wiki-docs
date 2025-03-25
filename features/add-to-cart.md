# Add to Cart

Nosto supports a quick-buy function straight from within its recommendations.

**Note 1:** The function definition is a part of the app and is automatically added to all pages. You do not need to define the function or include any additional scripts.

**Note 2:** BigCommerce variants are mapped to SKUs in Nosto. References to `variantId` below require the Nosto SKU id to be set.

In order to do this, two arguments must be passed to the `Nosto.addProductToCart` method.

The **first argument** is a javascript object containing the ID of the configurable product and the variant id, and **the second** one being the element where the product was added to the cart. If configurable product's id is 123 and the id of the variant is 124 the method call in recommendations template would look as follows

```javascript
Nosto.addProductToCart({productId: '123', variantId: '124'}, this)
```

### Leveraging Quantities

There is also the possibility to specify the quantity when adding products to cart:

```javascript
Nosto.addProductToCart({productId: '425', variantId: '310'}, this, 5)
```

### Adding Multiple Products

Furthermore you can add multiple products to the cart by using an array of objects containing the **productId** and the **variantId**. The product id is the parent product that holds the variations, and the variantId is the id of the BigCommerce variation (or Nosto SKU id). The parent id is needed in order to recognize which product was added and improve the recommendations.

```javascript
Nosto.addMultipleProductsToCart([
    {productId: '123', variantId: '124'}, 
    {productId: '425', variantId: '310'}
], this);
```

## Use Cases

A common way to implement the functionality to add a variation directly to cart is to implement a drop down of available variations for recommended products. The selected variation id is then picked up from the drop down and passed to the `Nosto.addProductToCart` method. An example implementation can be found below.

```markup
#if($product.skus.size() > 0)
  <div class="nosto-sku-select-wrapper">
    <label class="nosto-sku-select">
      <select onChange="document.getElementById('selected_sku').value = this.value">
        <option value="">Choose</option>
          #foreach($sku in $product.skus)
            <option value="$!sku.id">
              #foreach($attr in $sku.customFields)
                $!attr.key: $!attr.value
              #end
            </option>
          #end
      </select>
    </label>
  </div>
  <button onclick="Nosto.addProductToCart({productId: '$!product.productId', variantId: document.getElementById('selected_sku').value}, this);return false;" class="nosto-btn">
    <img src="$!props.display.buyButtonIcon.url" width="15" height="15">
  </button>
#end
```



A common challenge arises when you want to display **minicart recommendations** that are relevant to the current state of the cart. Ideally, you want the recommendations to reflect the **latest items the customer has added**.

To achieve this, when a customer adds a product to the cart proceed as follows:

1. **Fetch the updated cart data.**
2. **Pass the cart context to Nosto.**
3. **Request and display updated recommendations.**

This ensures that the suggestions shown in the minicart are timely and relevant.\
You can follow the example snippet below to implement this flow effectively.

```
fetch('/api/storefront/cart', {
      method: 'GET',
      credentials: 'same-origin'
    })
        .then(response => response.json())
        .then(carts => {
          if (carts.length > 0) {
            const cart = carts[0];
            const cartItems = cart.lineItems.physicalItems.concat(cart.lineItems.digitalItems);
            const nostoItems = cartItems.map(item => ({
              product_id: item.productId,
              sku_id: item.sku,
              quantity: item.quantity,
              name: item.name,
              unit_price: item.salePrice,
              price_currency_code: cart.currency.code
            }));
            Nosto.cart({ items: nostoItems });
          }
        })
        .catch(error => {
            console.error('Error fetching cart:', error);
        })
        .finally(() =>  Nosto.load());
```

