---
description: >-
  This page describes some helpful Javascript functions that allow you to manage
  the cart.
---

# Managing the Cart

### Add Recommended Products to Cart

Nosto supports a quick-buy function straight from within its recommendations. This feature will be referred to as the add-to-cart feature henceforth.

**Note:** The function definition is a part of the app and is automatically added to all pages. You do not need to define the function or include any additional scripts.

In order to do this, you two arguments must be passed to the `Nosto.addSkuToCart` method.

The first argument is a javascript object containing the id of the configurable product and the variation, and the second one is the `resultId` value from the response of the  `ev1` request that rendered the product . If configurable product's id is 123, the id of the variation is 124 and the `resultId` is `frontpage-nosto-1` , the method call in recommendations template would look like this,

```javascript
Nosto.addSkuToCart({productId: '123', skuId: '124'}, 'frontpage-nosto-1')
```

If you wish to combine cart mutation and recommendation reloading the following pattern should be used:

```javascript
async function addToCart(productId, skuId, attribution) {
  await Nosto.addSkuToCart({ productid, skuId }, attribution)
  const api = await new Promise(nostojs)
  await api.loadRecommendations()
}
```

or to reload only a specific recommendation

```javascript
async function addToCart(productId, skuId, attribution, toReload) {
  await Nosto.addSkuToCart({ productid, skuId }, attribution)
  const api = await new Promise(nostojs)
  await api.createRecommendationRequest({ includeTagging: true })
    .setElements([toReload])
    .load()

}
```

#### Leveraging Quantities

It is possible to specify the quantity when adding products to cart. The second parameter (`productpage-nosto-1`) is the `resultId` value form the `ev1` response.

```javascript
Nosto.addSkuToCart({productId: '425', skuId: '310'}, 'productpage-nosto-1', 5)
```

#### Adding Multiple Products

It is also possible to add multiple products to the cart by using an array of objects containing the productId, skuId and quantity. The product id is the parent product that holds the variations, and the SKU is the variation itself. The parent id is needed in order to recognize which product was added and improve the recommendations. Note that if you use a configurable product as SKU ID, will simply not be added to the cart, since you need to select the options.

```javascript
Nosto.addMultipleProductsToCart(
    [
        {productId:"1234", skuId:"4321", quantity: 1},
        {productId:"345", skuId:"543", quantity: 1}
    ], 
    'categorypage-nosto-1' 
)
```

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
  <button onclick="Nosto.addSkuToCart({productId: '$!product.productId', skuId: document.getElementById('selected_sku').value}, '$!product.attributionKey'); return false;" class="nosto-btn">
    <img src="$!props.display.buyButtonIcon.url" width="15" height="15">
  </button>
#end
```

Below is the example of how to add an SKU to cart where your product has a single SKU or you want to simply add the product's first SKU to cart instead of allowing the user to select which SKU to add.

```html
<a
  href="#"
  onclick="window.Nosto.addProductToCart('$!product.lastPathOfProductUrl()', '$!product.attributionKey');">
  Add to Cart
</a>
```

## Migrating from the Legacy approach

The older approach of APIs required `this` object representing the element that triggered the API call. For example, in the below example, `addSkuToCart` api takes `this`  object as second parameter. Nosto internally extracted the `resultId` from the object.

```javascript
Nosto.addSkuToCart({productId: '123', skuId: '124'}, this)
```

With the newer approach, the `addSkuToCart`  api has been updated to accept the `resultId` directly to improve effectiveness of the api and at the same time ensures proper attribution.

```javascript
Nosto.addSkuToCart({productId: '123', skuId: '124'}, 'productpage-nosto-1')
```

Similarly, the second parameter for `addProductToCart` and `addMultipleProductsToCart` APIs  can be updated to send the `resultId` value instead of the `this` object

```javascript
Nosto.addProductToCart('$!product.lastPathOfProductUrl()', 'productpage-nosto-1')
```

```javascript
Nosto.addMultipleProductsToCart(
    [
        {productId:"1234", skuId:"4321", quantity: 1},
        {productId:"345", skuId:"543", quantity: 1}
    ], 
    'categorypage-nosto-1' 
)
```

When invoking these APIs from Nosto's recommendation template, use the `$!product.attributionKey`  context value to send the `resultId` instead of the `this` object
