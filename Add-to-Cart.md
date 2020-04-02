Nosto supports a quick-buy function straight from within its recommendations. This feature will be referred to as the add-to-cart feature henceforth. As of version 3.0.0, it's possible to add variations (configurable products) directly to cart from recommendations.

![3.0.0](https://img.shields.io/badge/nosto-3.0.0-green.svg)

### Adding SKUs (variations) to cart
![3.1.0](https://img.shields.io/badge/nosto-3.1.0-green.svg)

**Note:** The function definition is a part of the extension and is automatically added to all pages. You do not need to define the function or include any additional scripts.

In order to do this, you two arguments must be passed to the `Nosto.addSkuToCart` method.

The first argument is a javascript object containing the id of the configurable product and the variation, and the second one being the element where the product was added to the cart. If configurable product's id is 123 and the id of the variation is 124 the method call in recommendations template would look like this

```javascript
Nosto.addSkuToCart({productId: '123', skuId: '124'}, this)
```

### Leveraging Quantities

![3.8.0](https://img.shields.io/badge/nosto-3.8.0-green.svg)

As of version `3.8.0` you can also pass the quantity as a parameter.

```javascript
Nosto.addSkuToCart({productId: '425', skuId: '310'}, this, 5)
```

### Adding Multiple Products

![3.8.0](https://img.shields.io/badge/nosto-3.8.0-green.svg)

As of version `3.8.0` it is also possible to add multiple products to the cart by using an array of objects containing the `productId` and the `skuId`.

The product id is the parent product that holds the variations, and the SKU is the variation itself. The parent id is needed in order to recognize which product was added and improve the recommendations.

Note that if you use a configurable product as SKU ID, will simply not be added to the cart and redirect you to the first configurable product page to select its options.

```javascript
Nosto.addMultipleProductsToCart([
        {'productId' : '380', 'skuId' : '380'},
        {'productId' : '381', 'skuId' : '381'},
        {'productId' : '382', 'skuId' : '382'},
        {'productId' : '383', 'skuId' : '383'},
        {'productId' : '385', 'skuId' : '385'}
    ], this);
```


## Use Cases

A common way to implement the functionality to add a variation directly to cart is to implement drop down of available variations for recommended products. The selected variation id is then picked up from the drop down and passed to the `Nosto.addSkuToCart` method. An example implementation can be found below.

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
  <button onclick="Nosto.addSkuToCart({productId: '$!product.productId', skuId: document.getElementById('selected_sku').value}, this);return false;" class="nosto-btn">
    <img src="$!props.display.buyButtonIcon.url" width="15" height="15">
  </button>
#end
```