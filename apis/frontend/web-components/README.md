# Web Components

[Nosto Web Components](https://github.com/Nosto/web-components) provides the necessary APIs to handle side-effects of a recommendation template like "Add to cart" button events, and other platform-specific APIs.

**Note**:\
This package provides headless web components. Templates must be provided by the user.

## Components

This package provides the following web components:

| Component                               | Category                  |
| --------------------------------------- | ------------------------- |
| [Campaign](./#campaign)       | Progressive Enhancement   |
| [Control](./#control)         | Templating   |
| [DynamicCard](./#dynamiccard) | Templating (Shopify only) |
| [Image](./#image)             | Progressive Enhancement   | 
| [Product](./#product)         | Progressive Enhancement   |
| [ProductCard](./#productcard) | Templating                |
| [SkuOptions](./#skuoptions)   | Progressive Enhancement   |

### `Campaign`

The `Campaign` custom element is a general-purpose solution for injecting or templating campaign results dynamically. It supports both HTML and JSON response modes and is designed for dynamic use cases like rendering nested campaign results.

#### Component attributes

| Attribute    | Description                                                                                                                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `placement`  | **Required.** Placement ID used to fetch campaign content (e.g. `frontpage-nosto-1`)                                                                                                                                            |
| `product-id` | Product ID for contextual recommendations. If provided, the campaign is scoped to that product                                                                                                                                  |
| `variant-id` | Reference to variant id. Refines the context to a specific product variant. Only used when `product-id` is provided                                                                                                             |
| `template`   | Name of the template to use. If provided, the campaign will use a JSON response and evaluate it using the given client-side template. If omitted, Nosto injects pre-rendered HTML from the backend directly into the component. |
| `init`       | For disabling automatic campaign loading on page load, set to `false`                                                                                                                                                           |

#### Usage example

**Example #1**:

Static campaign rendering:

```html
<nosto-campaign placement="best-sellers"></nosto-campaign>
```

**Example #2**:

Product-specific campaign:

```html
<nosto-campaign
  placement="best-sellers"
  product-id="123456"
  variant-id="sku-789">
</nosto-campaign>
```

**Example #3**:

Template-based rendering:

```html
<nosto-campaign placement="best-sellers">
  <template>
    <div class="product-card" v-for="product in products">
      <a :href="product.url">
        <img :src="product.imageUrl" />
      </a>
      <span class="product-name">{{ product.name }}</span>
      <span class="product-price">{{ product.price }}</span>
    </div>  
  </template>
</nosto-campaign>
```

A subset of Vue is used as the templating language. The template support is described in detail below.

### `Control`

The `Control` custom element provides conditional content rendering capabilities to inject customer segment specific content to the web page. The segment specific injections are defined as template children of the custom element.

The default content can be defined as follow up children of the custom element.

#### Usage example

```html
<nosto-control>
 <template segment="5a497a000000000000000001">New visitor content</template>,
 <template segment="5b71f1500000000000000006">Returning visitor content</template>
</nosto-control>
```

The content of the element will become `New visitor content` for new visitors and `Returning visitor content` for returning visitors.

### `DynamicCard`

`DynamicCard` is a custom element that delegates the product card rendering fully to Shopify with a given product handle and a reference to an alternate template to use. We recommend to skip the layout rendering in the alternative template.

This custom element is the recommended choice to use when the product card markup should be fully managed in Shopify templates instead of being spread to Shopify themes and Nosto templates.

#### Component attributes

| Attribute    | Description                           |
| ------------ | ------------------------------------- |
| `handle`     | Handle of the product                 |
| `template`   | Name of the alternate template to use |
| `section`    | Name of the product level section to use |
| `variant-id` | Optional reference to variant id      |
| `lazy`       | Optional lazy loading mode to delay loading until element is visible in viewport |

#### Usage example

The `DynamicCard` component relies on alternate product card templates to be exposed from Shopify. Here are example instructions for the Dawn theme:

* Identify the product grid section of the collection template
  * `main-collection-product-grid in Dawn`
* Identify the product card snippet in the product grid section
  * `card-product in Dawn`
* Copy the card snippet usage into a new product template (e.g. `product.card.liquid` under `templates`)

In addition to template targeting `DynamicCard` supports also the targeting of sections, in case you want to expose the product card as a section instead of a custom template.

```markup
{% layout none %}
{% render 'card-product', card_product: product %}
```

Make sure that web components are enabled in the Nosto Recommendation Settings after completion of the Shopify side changes.\
After that the component can be used in Nosto templates like this

```markup
#foreach($product in $products)
<nosto-dynamic-card handle="$!product.handle" template="card">
  <div class="product-card-skeleton"></div>
</nosto-dynamic-card>
#end
```

### `Image`

`Image` is a web component that provides response image rendering capabilities using the [unpic](https://unpic.pics/about/) library. It supports Shopify and BigCommerce thumbnails out of the box and uses the same configuration model and unpic's own web components. It can be used as a `img` element replacement with smart rendering capabilities.

#### Component attributes

| Attribute    | Description                           |
| ------------ | ------------------------------------- |
| `src`        | The source URL of the image.          |
| `width`      | The width of the image in pixels.     |
| `height`     | The height of the image in pixels.    |
| `aspect-ratio`  | The aspect ratio of the image (width / height value).      |
| `crop`       | Shopify only. The crop of the image. Can be "center", "left", "right", "top", or "bottom".      |
| `layout`     | The layout of the image. Can be "fixed", "constrained", or "fullWidth". |

**Example #1**:

Usage with Shopify image URL:

```html
<nosto-image src="https://cdn.shopify.com/static/sample-images/bath.jpeg" width="800" height="600" layout="constrained" crop="center"></nosto-image>
```

**Example #2**:

Usage with BigCommerce image URL:

```html
<nosto-image src="https://cdn11.bigcommerce.com/s-hm8pjhul3k/products/4055/images/23603/7-15297__04892.1719977920.1280.1280.jpg" width="800" height="600" layout="constrained"></nosto-image>
```

### `Product`

When markup (HTML) for rendering a product is wrapped with the `Product` component, the APIs for SKU selection and Add to cart functionality are automatically handled by the component. By encapsulating the necessary APIs, this component reduces any JavaScript logic that would otherwise be included in the template and helps the team to concentrate only on building the template rather than implementing the JavaScript logic.

#### Component attributes

Two mandatory component attributes:

| Attribute    | Description                                                                                                                |
| ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| `product-id` | Id of the product being rendered. `$!product.productId` provides the Product Id in templates.                              |
| `reco-id`    | The Id of the recommendation being rendered. `$!product.attributionKey` provides the Recommendation Id in templates.       |
| `n-sku-data` | To be applied on an optional script element with SKU data as a JSON array of { price, listPrice, image, altImage } entries |

**Note**:\
The following examples of rendering product SKUs are applicable only for simple use-cases. For complex cases, like multi-directional SKU selections where selecting color renders the matching size and vice-versa, consider using the `SkuOptions` component.

**Example #1**:

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

**Example #2**:

Render Product with individual SKU item acting as "Add to cart" button.

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

#### Markup Attributes

This component requires the following attributes to parse the markup, extract product & SKU data and attach the event handlers to the selector/button elements:

| Attribute        | Description                                                                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `n-sku-selector` | Marks the SKU select dropdown. Attaches an `onchange` event to the element. Clicking on the "Add to cart" button adds the SKU value selected from the dropdown to the cart. |
| `n-sku-id`       | Relevant when SKU options are rendered as "Add to cart" button. Supplies the ID of the SKU option value and should be supplied on the parent of "Add to cart" button.       |
| `n-img`          | Image URL for SKU which will be applied to Product wrapper on click                                                                                                    |
| `n-alt-img`      | Alternate image URL for SKU which will be applied to Product wrapper on                                                                                                |
| `n-price`        | Price for SKU which will be applied to Product wrapper on click                                                                                                        |
| `n-list-price`   | List price for SKU which will be applied to Product wrapper price on click                                                                                             |

```html
<div n-sku-id="456">
  <span n-atc>Blue</span>
</div>
```

`n-atc`\
Marks an element as Add to cart trigger and attaches click event to the element. Clicking on this element triggers `addSkuToCart` API and supplies the selected SKU Id.

### `ProductCard`

The `ProductCard` component acts as a basic product card component where the content is rendered via an externally defined template. The data is embedded via an inner script element with JSON contents and rendering happens via an embedded Vue-like compiler using an external template element.

Unlike `Product`, this component doesn't include any side effects or platform-specific API support on top of the rendered markup. For side effects the `wrap` attribute can be used to wrap the inner content in a `Product` instance.

```html
<nosto-product-card reco-id="789011" template="product-card-template">
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

### `SkuOptions`

The `SkuOptions` component is recommended for cases rendering multiple SKU option groups (color, size). It manages the state and interactions of SKU options, including pre-selection, state changes, and click events.

#### Component attribute

Requires one mandatory attribute

| Attribute | Description                                                                                                                   |
| --------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `name`    | A required attribute supplied on the `nosto-sku-options` element. Supplies the option group name (color/size/material etc...) |

**Example #1**:

Dual SKU selection group

```html
<nosto-product product-id="1223456" reco-id="789011">
  <nosto-sku-options name="color">
    <span n-option n-skus="123,145">Black</span>
    <span n-option n-skus="223,234,245">White</span>
    <span n-option n-skus="334,345">Blue</span>
  </nosto-sku-options>
  <nosto-sku-options name="size">
    <span n-option n-skus="123,223">L</span>
    <span n-option n-skus="234,334">M</span>
    <span n-option n-skus="145,245,345">S</span>
  </nosto-sku-options>
  <span n-atc>Add to cart</span>
</nosto-product>
```

**Example #2**:

Trio SKU selection group

```html
<nosto-product product-id="1223456" reco-id="789011">
  <nosto-sku-options name="color">
    <span n-option n-skus="123,145">Black</span>
    <span n-option n-skus="223,234,245">White</span>
    <span n-option n-skus="334,345">Blue</span>
  </nosto-sku-options>
  <nosto-sku-options name="size">
    <span n-option n-skus="123,223">L</span>
    <span n-option n-skus="234,334">M</span>
    <span n-option n-skus="145,245,345">S</span>
  </nosto-sku-options>
  <nosto-sku-options name="material">
    <span n-option n-skus="123,234,345">Cotton</span>
    <span n-option n-skus="145,223,334">Silk</span>
    <span n-option n-skus="245">Wool</span>
  </nosto-sku-options>
  <span n-atc>Add to cart</span>
</nosto-product>
```

**Example #3**:

Usage with select elements

```html
<nosto-product product-id="1223456" reco-id="789011">
  <nosto-sku-options name="color">
    <select n-target>
      <option value="black" n-skus="123,145">Black</option>
      <option value="white" n-skus="223,234,245">White</option>
      <option value="blue" n-skus="334,345">Blue</option>
    </select>
  </nosto-sku-options>
  <nosto-sku-options name="size">
    <select n-target>
      <option value="l" n-skus="123,223">L</option>
      <option value="m" n-skus="234,334">M</option>
      <option value="s" n-skus="145,245,345">S</option>
    </select>
  </nosto-sku-options>
  <span n-atc>Add to cart</span>
</nosto-product>
```

#### Markup Attributes

| Attribute      | Description                                                                                                                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `n-option`     | Marks an element as SKU option element                                                                                                                                                           |
| `n-skus`       | Comma-separated value of linked available SKU Ids. `$!product.getSkuAggregateOptions` method in templates provides the Sku aggregates for the supplied custom field (color/size/material etc...) |
| `n-skus-oos`   | Comma-separated value of linked unavailable SKU Ids. The usage of this parameter is optional and should be considered when Out of stock SKUs should be considered.                               |
| `n-img`        | Image URL for SKU option which will be applied to Product wrapper on click                                                                                                                  |
| `n-alt-img`    | Alternate image URL for SKU option which will be applied to Product wrapper on click                                                                                                        |
| `n-price`      | Price for SKU option which will be applied to Product wrapper on click                                                                                                                      |
| `n-list-price` | List price for SKU option which will be applied to Product wrapper price on click                                                                                                           |

Disabled options that are not available due to selections in other groups are marked with the `disabled` attribute and unavailable options that are Out of stock are marked with the `unavailable` attribute. Both should be styled distinctively.

## Pre-selected options

In addition to handling APIs, supports pre-selection of SKU options. For example in the template below, option `SKU 2` is rendered with `selected` attribute. With this template, when clicking "Add to cart" button, `SKU 2` gets added to the cart.

```html
<nosto-product product-id="123456" reco-id="789011">
  ...
  <select n-sku-selector>
    <option value="456">SKU 1</option>
    <option value="457" selected>SKU 2</option>
  </select>
  ...
  <button n-atc>Add to cart</button>
  ...
</nosto-product>
```

with `nosto-sku-options` component

```html
<nosto-product product-id="123456" reco-id="789011">
  ...
  <nosto-sku-options name="colors">
    <span black n-option n-skus="123,145" selected>Black</span>
    <span white n-option n-skus="223,234,245">White</span>
    <span blue n-option n-skus="334,345">Blue</span>
  </nosto-sku-options>
  ...
</nosto-product>
```

Note:\
The component does not handle styling for selected options and it has to be applied from the template.

## Default disabled

This is useful when some SKU options are to be hidden or greyed out. The `disabled` attribute represents an unsupported option and is added to all unsupported SKU options on SKU selections. This is useful when SKU options that are Out-Of-Stock needs to be hidden or greyed out. By default, the `disabled` attribute is added to all unsupported SKU options. For example, when there is no `M` size in `Blue` color, on selection of `Blue` color, the component adds `disabled` attribute to option `M`.

Note:\
This functionality is applicable only when using `nosto-sku-options` component\
The component does not handle styling for disabled options and it has to be applied from the template.

```html
<nosto-product product-id="123456" reco-id="789011">
  ...
  <nosto-sku-options name="colors">
    <span black n-option n-skus="123,145">Black</span>
    <span white n-option n-skus="223,234,245" disabled>White</span>
    <span blue n-option n-skus="334,345">Blue</span>
  </nosto-sku-options>
  ...
</nosto-product>
```

## Vue-like templating

`Campaign` and `ProductCard` support a subset of Vue templating as the templating language. The supported features are mustache interpolation and the directives that are listed below. Reactivity is not supported.

* [v-text](https://vuejs.org/api/built-in-directives.html#v-text)
* [v-html](https://vuejs.org/api/built-in-directives.html#v-html)
* [v-show](https://vuejs.org/api/built-in-directives.html#v-show)
* [v-if](https://vuejs.org/api/built-in-directives.html#v-if)
* [v-else](https://vuejs.org/api/built-in-directives.html#v-else)
* [v-else-if](https://vuejs.org/api/built-in-directives.html#v-else-if)
* [v-for](https://vuejs.org/api/built-in-directives.html#v-for)
* [v-bind](https://vuejs.org/api/built-in-directives.html#v-bind) (including shorthand syntax)
  * modifiers are not supported
* [v-on](https://vuejs.org/api/built-in-directives.html#v-on)
  * modifiers are not supported
* [v-pre](https://vuejs.org/api/built-in-directives.html#v-pre)    
* [v-cloak](https://vuejs.org/api/built-in-directives.html#v-cloak)

For the documentation of these directives the [Vue reference docs](https://vuejs.org/api/built-in-directives.html) is a good starting point.
