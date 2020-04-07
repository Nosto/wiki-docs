# Add to Cart

Nosto supports a quick-buy function straight from within its recommendations. This feature will be referred to as the add-to-cart feature henceforth.

**Note:** The function definition is a part of the app and is automatically added to all pages. You do not need to define the function or include any additional scripts.

In order to do this, you two arguments must be passed to the `Nosto.addSkuToCart` method.

The first argument is a javascript object containing the id of the configurable product and the variation, and the second one being the element where the product was added to the cart. If configurable product's id is 123 and the id of the variation is 124 the method call in recommendations template would look like this

```javascript
Nosto.addSkuToCart({productId: '123', skuId: '124'}, this)
```

### Leveraging Quantities

It is possible to specify the quantity when adding products to cart.

```javascript
Nosto.addSkuToCart({productId: '425', skuId: '310'}, this, 5)
```

### Adding Multiple Products

It is not possible to add multiple products to the cart at once. This is a limitation of Bigcommerce Ajax API and the only way to add multiple products to the cart at once is by invoking the add-to-cart function multiple times.

## Use Cases

A common way to implement the functionality to add a variation directly to cart is to implement drop down of available variations for recommended products. The selected variation id is then picked up from the drop down and passed to the `Nosto.addSkuToCart` method. An example implementation can be found below.

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
  <button onclick="Nosto.addSkuToCart({productId: '$!product.productId', skuId: document.getElementById('selected_sku').value}, this);return false;" class="nosto-btn">
    <img src="$!props.display.buyButtonIcon.url" width="15" height="15">
  </button>
#end
```

