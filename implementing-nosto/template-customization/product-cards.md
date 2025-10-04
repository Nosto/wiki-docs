# Product cards

Nosto recommends using shop-provided resources for rendering product cards in Nosto templates. Doing so offers:

* Faster onboarding
* Easier maintenance
* Consistent styling and behavior

Below are the recommended approaches.

## Custom Web Components

If your shop themes use web components, we suggest leveraging them in your Nosto templates as well. This avoids duplicating markup and logic between your shop and Nosto templates. For building web components efficiently, consider using [Lit](https://lit.dev/) or similar high-level frameworks.

## Nosto Web Components

Nosto offers several web components designed to simplify product card integration:

* **DynamicCard**\
  Renders product cards entirely on the Shopify side.\
  &#xNAN;_&#x52;equires alternate product card templates to be available within Shopify themes._ Choose this approach if the shop already uses product card markup in Liquid templates and you want to reuse that markup in Nosto campaign rendering. Detailed instructions on how to set this up in your Shopify store are provided [here](https://docs.nosto.com/shopify/styling-options-dynamic-product-cards)
* **ProductCard**\
  Provides a platform-agnostic custom element that delegates rendering to shop side Vue-like templates. Choose this approach for non-Shopify merchants who prefer to manage product card templates within the shop’s own templates, rather than using Nosto’s templates.
* **Product**\
  Enhances static product card markup with interactive features such as:
  * Swatch selection
  * Add-to-cart interactions
  * Dynamic product image updates based on swatch and SKU selections

### Syntax Examples

#### DynamicCard

Basic usage with template:

```html
#foreach($product in $products)
<nosto-dynamic-card handle="$!product.handle" template="card">
  <div class="product-card-skeleton"></div>
</nosto-dynamic-card>
#end
```

Using with section and placeholder:

```html
#foreach($product in $products)
<nosto-dynamic-card handle="$!product.handle" section="product-card-section" placeholder lazy>
  <div class="loading-placeholder">Loading...</div>
</nosto-dynamic-card>
#end
```

#### ProductCard

Using with embedded JSON data:

```html
<nosto-product-card template="product-card-template">
  <script type="application/json" product-data>
    {
      "id": "1223456",
      "image": "https://example.com/images/awesome-product.jpg",
      "title": "Awesome Product",
      "price": "19.99",
      "listPrice": "29.99"
    }
  </script>
</nosto-product-card>

<template id="product-card-template">
  <img :src="product.image" :alt="product.title" class="product-image" />
  <h1>{{ product.title }}</h1>
  <p class="price">
    <span n-price>{{ product.price }}</span>
  </p>
  <p class="list-price" v-if="product.price !== product.listPrice">
    <span n-list-price>{{ product.listPrice }}</span>
  </p>
</template>
```

#### Product

Render Product with SKU selection dropdown and an "Add to cart" button:

```html
<nosto-product product-id="123456" reco-id="789011">
  ...
  <select n-sku-selector>
    <option value="456">SKU 1</option>
    <option value="457">SKU 2</option>
  </select>
  ...
  <div n-atc>Add to cart</div>
  ...
</nosto-product>
```

Render Product with individual SKU item acting as "Add to cart" button:

```html
<nosto-product product-id="123456" reco-id="789011">
  ...
  <div n-sku-id="456">
    <span n-atc>Blue</span>
  </div>
  <div n-sku-id="101">
    <span n-atc>Black</span>
  </div>
  ...
</nosto-product>
```

Nosto's web component offering is documented [here](../../apis/frontend/web-components/)

## Style Reuse

If web components aren’t an option, we advise duplicating only the markup for product cards within Nosto templates while applying shop-side CSS rules to maintain consistent styling.
