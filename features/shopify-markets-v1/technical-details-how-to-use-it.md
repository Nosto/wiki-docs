# Technical Details (how to use it)

### Before you start

In order to workaround product procing for Shopify Markets, there are two small changes needed in your Shopify code.

1.  You need to manually **remove** or **comment out** the following code-snipped from `nosto-tagging.liquid`:\


    ```liquid
    {% raw %}
    {% if shop.enabled_currencies.size > 1 %}
    {% for currency in shop.enabled_currencies %}
    {% if currency == cart.currency %}
    <div class="nosto_variation" style="display: none;">{{ currency.iso_code }}</div>
    {% endif %}
    {% endfor %}
    {% endif %}
    {% endraw %}
    ```
2.  You need to manually **add** the following snipped to your `theme.liquid`: \


    ```liquid
    <script>
      var Shopify = Shopify || {};
      Shopify.nosto = {}
      Shopify.nosto.money_format = {{ shop.money_format | json }};
    </script
    ```
3.  **Deprecated Approach**: You need to manually **add** the following snipped to your `theme.liquid`: \


    ```liquid
    <script>
      var Shopify = Shopify || {};
      Shopify.money_format = {{ shop.money_format | json }};
    </script
    ```

### Usage

The API takes an Object `RecProductElementSelectors` as a single parameter. The following table lists all the allowed mandatory and optional parameters of `RecProductElementSelectors` Object

<table><thead><tr><th width="240" align="center">Parameter Name</th><th align="center">Required/Optional</th><th align="center">Type</th><th>Description</th></tr></thead><tbody><tr><td align="center">productSectionElement</td><td align="center">Required</td><td align="center">String</td><td>CSS selector of the element containing all the product details and the variant options</td></tr><tr><td align="center">productUrlElement</td><td align="center">Required</td><td align="center">String</td><td>CSS selector of all <code>a</code> elements within <code>productSectionElement</code> containing the product URL</td></tr><tr><td align="center">productTitleElement</td><td align="center">Required</td><td align="center">String</td><td>CSS selector of all elements within the <code>productSectionElement</code> element containing the product title</td></tr><tr><td align="center">productHandleAttribute</td><td align="center">Required</td><td align="center">String</td><td>Attribute name for fetching the product handle. This value can be fetched in recommendation template using <code>$!product.lastPathOfProductUrl</code>. This attribute has to be part of the same element referenced by <code>productSectionElement</code></td></tr><tr><td align="center">priceElement</td><td align="center">Required</td><td align="center">String</td><td>CSS selector of all elements within the <code>productSectionElement</code> element containing the product price</td></tr><tr><td align="center">listPriceElement</td><td align="center">Optional</td><td align="center">String</td><td>CSS selector of all elements within the <code>productSectionElement</code> element containing the product list price. This is used only when the product has a discounted price and the recommendation displays both old &#x26; new price.</td></tr><tr><td align="center">defaultVariantIdAttribute</td><td align="center">Optional</td><td align="center">String</td><td>Attribute name for selecting the product variant. The first product variant is used if this is not provided. This attribute has to be part of the same element referenced by <code>productSectionElement</code>.</td></tr><tr><td align="center">descriptionElement</td><td align="center">Optional</td><td align="center">String</td><td>CSS selector of all elements within the <code>productSectionElement</code> element containing the product description</td></tr><tr><td align="center">totalVariantOptions</td><td align="center">Optional</td><td align="center">Number</td><td>Number of variant options used in the recommendation template. Useful when the recommendation doesn't display all the product variant options in the recommendation</td></tr><tr><td align="center">variantSelector</td><td align="center">Optional</td><td align="center">Object/VariantOptionSelector</td><td>Needed only when recommendations display variant options. More on this in the next section</td></tr></tbody></table>

The following table lists all the allowed mandatory and optional parameters of `VariantOptionSelector` Object. This parameter is required only when the recommendation displays variant options.

|      Parameter Name      | Required/Optional |             Type             | Description                                                                                                                                                                                                                                                                                                                                                                     |
| :----------------------: | :---------------: | :--------------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       titleElement       |      Optional     |            String            | CSS classname selector/Id of the element hosting the variant option title. Either this or `optionElement` selector must be provided. Both are not mandatory. Title element won't be updated when this option is ignored                                                                                                                                                         |
|       optionElement      |      Optional     |            String            | CSS classname selector of all the elements hosting the variant option values. This selector should be in-depth such that it directly points to the element hosting the option value. Either this or `titleElement` selector must be provided. Both are not mandatory. Option elements won't be updated when this option is ignored                                              |
|      selectedElement     |      Optional     |            String            | Must be provided when `optionElement` selector is provided. CSS selector/id of the element selected in the variant option. In case of multiple variant options, It's required to use the same selector across all the variant options to avoid any issues.                                                                                                                      |
|         position         |      Optional     |            Number            | Must be provided when `optionElement` selector is provided. Numeric value of the order number/position of the variant option as declared in the Shopify admin product page. It's not required to display the elements, in the same order, in the recommendations, but it's requred to supply the appropriate position number and it should match the order in the Shopify admin |
| dependentVariantSelector |      Optional     | Object/VariantOptionSelector | Selectors configuration for sub-level variants that changes based on the value selected in this primary variant. A maximum of 5 sub-level variants can be supported                                                                                                                                                                                                             |

### Error reporting

In case of any issues with the API call or element structuring, the API will promptly report the errors in the browser console. Please contact Nosto support in case you come across these issues.

### Limitations

1. Only allows upto 5 sub-level variants
2. Only allows 1 primary variant
3. Strict structuring necessary. In other words, it's necessary to follow the rules defined in the description of each API parameters.
4. While providing multiple variant options, it's not possible to pre-configure the option to be selected for sub-level variants. The API selects the first option automatically.

### Examples

There are two ways of using the JS API and it totally depends on the content of recommendation template.

1. Without variant options
2. With variant options

In this section, we discuss about both the approaches with an example

**Using API without variant options**

{% hint style="info" %}
Only the necessary part of the template is shown below. It's not a complete recommendation template.
{% endhint %}

```html
<div class="nosto-product-info" x-nosto-handle="$!product.lastPathOfProductUrl()" x-default-variant="$!sku.id">
    <span class="x-nosto-title">$!product.name</span>
    <span class="x-nosto-desc">$!product.description</span>
    <span class="x-nosto-title">$!product.name</span>

    <a class="nosto-product-url" href="$!product.url" alt="$!product.name">
        <div class="nosto-image-container img-$imgCount"></div>
    </a>
      
    <a href="$!product.url" class="nosto-product-url nosto-product-brand">$!product.brand.truncate(100)</a>
    <a href="$!product.url" class="nosto-product-url nosto-product-name">$!product.name.truncate(100)</a>

    <a href="$!product.url" class="nosto-product-url nosto-product-price">
        #if($product.discounted)
            <span class="nosto-oldprice money">$!product.listPrice</span>
        #end
        <span class="nosto-newprice money">$!product.price</span>
    </a>

    <div class="nosto-rating">
        #set($ratingInt = $math.round($!product.ratingValue.getAsNumber()).intValue())
        #set($ratingRest = 5 - $ratingInt)
        #if($ratingInt > 0)
            #foreach ($number in [1..$ratingInt])
            <span class="nosto-star-full">★</span>
            #end
        #end
        #if($ratingRest != 0)
            #foreach ($number in [1..$ratingRest])
                <span class="nosto-star-empty">★</span>
            #end
        #end

        <span class="nosto-reviews">($!product.reviewCount)</span>
    </div>
</div>
```

For the template above, you can call the JS API

With all the parameters as below:

```javascript
    _targetWindow.Nosto.migrateToShopifyMarket({
            productSectionElement: '.nosto-product-info',
            productTitleElement: '.x-nosto-title',
            productUrlElement: '.nosto-product-url',
            defaultVariantIdAttribute: 'x-default-variant',
            productHandleAttribute: 'x-nosto-handle',
            priceElement: '.nosto-product-price > .nosto-newprice.money',
            listPriceElement: '.nosto-product-price > .nosto-oldprice money.money',
            descriptionElement: 'x-nosto-desc',
        })
```

With only the necessary parameters as below:

```javascript
_targetWindow.Nosto.migrateToShopifyMarket({
            productSectionElement: '.nosto-product-info',
            productUrlElement: '.nosto-product-url',
            productHandleAttribute: 'x-nosto-handle',
            priceElement: '.nosto-product-price > .money',
        })
```

**Using API with variant options**

{% hint style="info" %}
Only the necessary part of the template is shown below. It's not a complete recommendation template. While loading the variant options, is not necessary to load the values for the sub-variants. The API loads the data automatically for the selected market and language
{% endhint %}

```html
<div class="nosto-product-url nosto-sm-product-info" x-nosto-handle="$!product.lastPathOfProductUrl()">
    <span class="x-nosto-title">$!product.name</span>
    <span class="x-nosto-desc">$!product.description</span>
    <span class="x-nosto-title">$!product.name</span>

    <a class="nosto-product-url" href="$!product.url" alt="$!product.name">
        <div class="nosto-image-container img-$imgCount"></div>
    </a>

    <a href="$!product.url" class="nosto-product-brand">$!product.brand.truncate(100)</a>
    <a href="$!product.url" class="nosto-product-url nosto-product-name">$!product.name.truncate(100)</a>
    <a href="$!product.url" class="nosto-product-url nosto-product-price">
        #if($product.discounted)
            <span class="nosto-oldprice money">$!product.listPrice</span>
        #end
        <span class="nosto-newprice money">$!product.price</span>
    </a>
    <div class="nosto-rating">
        #set($ratingInt = $math.round($!product.ratingValue.getAsNumber()).intValue())
        #set($ratingRest = 5 - $ratingInt)
        #if($ratingInt > 0)
            #foreach ($number in [1..$ratingInt])
                <span class="nosto-star-full">★</span>
            #end
        #end

        #if($ratingRest != 0)
            #foreach ($number in [1..$ratingRest])
                <span class="nosto-star-empty">★</span>
            #end
        #end

        <span class="nosto-reviews">($!product.reviewCount)</span>
    </div>
    
    <div class="variant-options">
        <h3 class="size-option-title">Size</h3>
        <div class="nosto-sizes">
            #foreach($sku in $product.skus)
                <span x-nosto-product="$!product.productId" class="nosto-size #if(!$!sku.available) nosto-disabled #end">$!sku.getCustomField('Size')</span>
            #end
        </div>
        <h3 class="color-option-title">Color</h3>
        <div class="nosto-colors">
        </div>
    </div>
</div>
```

For the template above, you can call the JS API

```javascript
(function() {
        
        const win = _targetWindow
        const doc = _targetWindow.document
        
            const nostoProducts = [
                    #foreach($product in $products)
                            {
                                productId : '$!product.productId',
                                image : '$!product.thumb(8)',
                                name : '$!product.name',
                                price : "$!product.price", 
                                skus: [
                                        #foreach($sku in $product.skus)
                                            {
                                                skuId : '$!sku.id',
                                                available: $!sku.available,
                                                size: '$!sku.getCustomField("Size")',
                                                color: '$!sku.getCustomField("Color")'
                                            },
                                        #end
                                    ]
                            },
                    #end
                ]
            
            
            
            function loadColor(product, ele) {
                const sizeSku = ele.innerHTML
                
                const wrapperElement = ele.closest('.variant-options')
                const parentElement = wrapperElement.querySelector('.nosto-colors')
                parentElement.innerHTML = ""
                
                const colorElements = product.skus.filter(sku => sku.size === sizeSku)
                    .map(filteredSku => {
                        const colorElement = document.createElement('span')
                        colorElement.classList.add('nosto-color')
                        if(!filteredSku.available) 
                            colorElement.classList.add('nosto-disabled')
                            
                        colorElement.innerHTML = '&nbsp;'
                      
                        parentElement.append(colorElement)
                        return colorElement
                    })
                
                triggerMigration()
                
            }
            
            doc.querySelectorAll('.nosto-sm-product-info')
                    .forEach(np => {
            np.querySelectorAll('.nosto-sizes > .nosto-size:not(.nosto-disabled)')
                            .forEach(ele => { 
                                ele.onclick = function(e) {
                                    e.preventDefault()
                                    clearSelectedVariant(np)
                                    ele.classList.add('selected')
                                    const prodId = ele.getAttribute('x-nosto-product')
                                    const matchedProd = nostoProducts.find(product => product.productId == prodId)
                                    loadColor(matchedProd, ele)
                                }   
                            })
                        
                        const initialVariant = np.querySelector('.nosto-sizes > .nosto-size:not(.nosto-disabled)')
                        initialVariant.classList.add('selected')
                        const prodId = initialVariant.getAttribute('x-nosto-product')
                        const matchedProd = nostoProducts.find(product => product.productId == prodId)
                        loadColor(matchedProd, initialVariant)
                    
                    })
                    
                    
            function clearSelectedVariant(np) {
                np.querySelectorAll('.nosto-sizes > .nosto-size:not(.nosto-disabled)')
                .forEach(ele => ele.classList.remove('selected'))
            }
                    
            
            triggerMigration()
    })()
```

`triggerMigration` method declared in the above example invokes the JS API,

With all the parameters as below:

```javascript
function triggerMigration() {
    return win.Nosto.migrateToShopifyMarket({
        productSectionElement: '.nosto-sm-product-info',
        productTitleElement: '.x-nosto-title',
        descriptionElement: 'x-nosto-desc',
        productHandleAttribute: 'x-nosto-handle',
        priceElement: '.nosto-product-price > .nosto-newprice.money',
        listPriceElement: '.nosto-product-price > .nosto-oldprice.money',
        productTitleElement: '.nosto-product-name',
        totalVariantOptions: 2, /* When this value is incorrect, JS API will consider the actual variant option count in the product */
        variantSelector: {
            primaryVariantSelector: {
                titleElement: '.size-option-title',
                optionElement: '.nosto-sizes .nosto-size',
                position: 1,
                selectedElement: '.selected',
                dependentVariantSelector: {
                    titleElement: '.color-option-title',
                    optionElement: '.nosto-colors .nosto-color',
                    position: 2,
                    selectedElement: '.selected'
                }
            }
        }
    })
}
```

With only the necessary parameters as below:

```javascript
function triggerMigration() {
        return win.Nosto.migrateToShopifyMarket({
        productSectionElement: '.nosto-sm-product-info',
        productHandleAttribute: 'x-nosto-handle',
        priceElement: '.nosto-product-price > .nosto-newprice.money',
        productTitleElement: '.nosto-product-name',
        variantSelector: {
            primaryVariantSelector: {
                titleElement: '.size-option-title',
                optionElement: '.nosto-sizes .nosto-size',
                position: 1,
                selectedElement: '.selected',
                dependentVariantSelector: {
                    titleElement: '.color-option-title',
                    optionElement: '.nosto-colors .nosto-color',
                    position: 2,
                    selectedElement: '.selected'
                }
            }
        }
    })
}
```
