# Add to Cart

It is possible to add variations \(SKUs\) directly to cart from recommendations.

![3.0.0](https://img.shields.io/badge/nosto-3.0.0-green.svg)

**Note:** The function definition is a part of the extension and is automatically added to all pages. You do not need to define the function or include any additional scripts.

In order to do this, you must pass the product id of the actual variation `Nosto.addSkuToCart` method.

The first argument is the id of variation, and the second one being the element where the product was added to the cart. If configurable product's id is 123 and the id of the variation is 124 the method call in recommendations template would look like this

```javascript
Nosto.addProductToCart('124', this);
```

### Leveraging Quantities

![3.0.0](https://img.shields.io/badge/nosto-3.0.0-red.svg)

As of version `3.0.0` you can also pass the quantity as a parameter.

```javascript
Nosto.addSkuToCart({productId: '425', skuId: '310'}, this, 5)
```

### Adding Multiple Products

It is not possible to add multiple products to the cart at once. Please see issue [https://github.com/Nosto/nosto-prestashop/issues/263](https://github.com/Nosto/nosto-prestashop/issues/263)

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

