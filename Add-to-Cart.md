Nosto supports a quick-buy function straight from within its recommendations. This feature will be referred to as the add-to-cart feature henceforth. As of version 2.0.0, it's possible to add variations (configurable products) directly to cart from recommendations.

![2.0.0-RC1](https://img.shields.io/badge/nosto-2.0.0-green.svg)

**Note:** The function definition is a part of the extension and is automatically added to all pages. You do not need to define the function or include any additional scripts.

In order to do this, you must pass the product id of the actual variation `Nosto.addSkuToCart` method.

The first argument is the id of variation, and the second one being the element where the product was added to the cart. If configurable product's id is 123 and the id of the variation is 124 the method call in recommendations template would look like this

```javascript
Nosto.addProductToCart('124', this);
```

### Leveraging Quantities

![2.11.0](https://img.shields.io/badge/nosto-2.11.0-green.svg)

As of version `2.11.0` you can also pass the quantity as a parameter.

```javascript
Nosto.addProductToCart('124', this, 2);
```

### Adding Multiple Products

![2.12.0](https://img.shields.io/badge/nosto-2.12.0-red.svg)

It is also possible to add multiple products to the cart by using an array of objects containing the `productId` and the `skuId`.

The product id is the parent product that holds the variations, and the SKU is the variation itself.

The parent id is needed in order to recognize which product was added and improve the recommendations.

**Note** that if you use a configurable product as SKU ID, will simply not be added to the cart, since you need to select the options.

```javascript
Nosto.addMultipleProductsToCart([
        {'productId' : '123', 'skuId' : '123'},
        {'productId' : '124', 'skuId' : '124'},
        {'productId' : '111', 'skuId' : '222'}
    ], this);
```

## Use Cases

A common way to implement the functionality of adding variation directly to cart is to implement drop down of available variations for recommended products. The selected variation id is then picked up from the drop down and passed to the `Nosto.addSkuToCart` method. An example implementation can be found below.

```html
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
  <button onclick="Nosto.addProductToCart(document.getElementById('selected_sku').value, this);return false;" class="nosto-btn">
    <img src="$!props.display.buyButtonIcon.url" width="15" height="15">
  </button>
#end
```