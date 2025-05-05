# Web Components

[Nosto Web Components](https://github.com/Nosto/web-components) provides the necessary APIs to handle side-effects of a recommendation template like "Add to cart" button events, and other platform-specific APIs.

**Note**:\
This package doesn't render HTML markups on its own and the template should be provided by the user.

## Components

This package provides the following web components:

| Component                               | Status     |
| --------------------------------------- | ---------- |
| [NostoProduct](./#nostoproduct)         | PRODUCTION |
| [NostoProductCard](./#nostoproductcard) | ALPHA      |
| [NostoSkuOptions](./#nostoskuoptions)   | PRODUCTION |
| [NostoShopify](./#nostoshopify)         | ALPHA      |
| [NostoSwiper](./#nostoswiper)           | BETA       |

### `NostoProduct`

When markup (HTML) for rendering a product is wrapped with the `NostoProduct` component, the APIs for SKU selection and Add to cart functionality are automatically handled by the component. By encapsulating the necessary APIs, this component reduces any JavaScript logic that would otherwise be included in the template and helps the team to concentrate only on building the template rather than implementing the JavaScript logic.

#### Component attributes

Two mandatory component attributes:

| Attribute    | Description                                                                                                                |
| ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| `product-id` | Id of the product being rendered. `$!product.productId` provides the Product Id in templates.                              |
| `reco-id`    | The Id of the recommendation being rendered. `$!product.attributionKey` provides the Recommendation Id in templates.       |
| `n-sku-data` | To be applied on an optional script element with SKU data as a JSON array of { price, listPrice, image, altImage } entries |

**Note**:\
The following examples of rendering product SKUs are applicable only for simple use-cases. For complex cases, like multi-directional SKU selections where selecting color renders the matching size and vice-versa, consider using the `NostoSkuOptions` component.

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
| `n-img`          | Image URL for SKU which will be applied to NostoProduct wrapper on click                                                                                                    |
| `n-alt-img`      | Alternate image URL for SKU which will be applied to NostoProduct wrapper on                                                                                                |
| `n-price`        | Price for SKU which will be applied to NostoProduct wrapper on click                                                                                                        |
| `n-list-price`   | List price for SKU which will be applied to NostoProduct wrapper price on click                                                                                             |

```html
<div n-sku-id="456">
  <span n-atc>Blue</span>
</div>
```

`n-atc`\
Marks an element as Add to cart trigger and attaches click event to the element. Clicking on this element triggers `addSkuToCart` API and supplies the selected SKU Id.

### `NostoProductCard`

The `NostoProductCard` component acts as a basic product card component where the content is rendered via an externally defined template. The data is embedded via an inner script element with JSON contents and rendering happens via Liquid or Handlebars using an external template element.

Unlike `NostoProduct`, this component doesn't include any side effects or platform-specific API support on top of the rendered markup. For side effects the `wrap` attribute can be used to wrap the inner content in a `NostoProduct` instance.

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

<script id="product-card-template" type="text/x-liquid-template">
  <img src="{{ product.image }}" alt="{{ product.title }}" class="product-image" />
  <h1>{{ product.title }}</h1>
  <p class="price">
    <span n-price>{{ product.price }}</span>
  </p>
  <p class="list-price">
    <span n-list-price>{{ product.listPrice }}</span>
  </p>
</script>
```

### `NostoSkuOptions`

The `NostoSkuOptions` component is recommended for cases rendering multiple SKU option groups (color, size). It manages the state and interactions of SKU options, including pre-selection, state changes, and click events.

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
| `n-img`        | Image URL for SKU option which will be applied to NostoProduct wrapper on click                                                                                                                  |
| `n-alt-img`    | Alternate image URL for SKU option which will be applied to NostoProduct wrapper on click                                                                                                        |
| `n-price`      | Price for SKU option which will be applied to NostoProduct wrapper on click                                                                                                                      |
| `n-list-price` | List price for SKU option which will be applied to NostoProduct wrapper price on click                                                                                                           |

Disabled options that are not available due to selections in other groups are marked with the `disabled` attribute and unavailable options that are Out of stock are marked with the `unavailable` attribute. Both should be styled distinctively.

### `NostoSwiper`

Lightweight [Swiper](https://swiperjs.com/get-started) wrapper. The `NostoSwiper` component will load Swiper library on demand from CDN unless it is available as a direct dependency.

**Example**:

```html
<nosto-swiper>
  <div class="swiper-wrapper">
    <nosto-product product-id="123456" reco-id="78901"> ... </nosto-product>
  </div>
  <script type="application/json" swiper-config>
    {
      "direction": "horizontal",
      "loop": true,
      "slidesPerView": 3
    }
  </script>  
</nosto-swiper>
```

#### Modules

In order to use Swiper modules the module names to be loaded must be passed as an array.

```html
<nosto-swiper>
  <div class="swiper-wrapper">
    <!-- Swiper slides -->
  </div>  
  <script type="application/json" swiper-config>
    {
      "direction": "horizontal",
      "loop": true,
      "slidesPerView": 3,
      "modules": ["navigation"]
      "navigation": {
        "nextEl": ".swiper-button-next",
        "prevEl": ".swiper-button-prev"
      }
    }
  </script>
</nosto-swiper>
```

#### Markup attributes

| Attribute       | Description                                                                       |
| --------------- | --------------------------------------------------------------------------------- |
| `swiper-config` | Marks the `<script type="application/json">` child block as Swiper configuration. |
| `inject-css`    | To be used on NostoSwiper level to trigger loading of Swiper CSS                  |

### `NostoShopify`

A component wrapper for performing Shopify specific APIs. Variant options are not yet supported.

**Example**:

Migrate to Shopify market

```html
<div id="frontpage-nosto-1" class="nosto-element">
  <nosto-shopify markets>
    <nosto-product product-id="123456" reco-id="789011">
      <div class="product-card nosto-item" n-handle="5-pocket-jean">
        <span n-url="https://shopeasy-local.myshopify.com/products/5-pocket-jean"></span>
        <h3 n-title>5 Pocket Jean</h3>
        <span n-description> The 5 Pocket Jean by Nigel Cabourn is your denim go-to for every occasion. </span>
        <span class="product-price" n-price> 110$ </span>
        <span class="product-list-price" n-list-price> 110$ </span>
        <nosto-sku-options name="colors">
          <span n-option n-skus="123,145">Black</span>
          <span n-option n-skus="223,234,245">White</span>
          <span n-option n-skus="334,345">Blue</span>
        </nosto-sku-options>
        <nosto-sku-options name="sizes">
          <span n-option n-skus="123,223">L</span>
          <span n-option n-skus="234,334">M</span>
          <span n-option n-skus="145,245,345">S</span>
        </nosto-sku-options>
        <span n-atc>Add to cart</span>
      </div>
    </nosto-product>
  </nosto-shopify>
</div>
```

#### Markup attributes

| Attribute       | Description                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| `n-url`         | Product URL. `$product.url` provides the product URL in templates                                            |
| `n-title`       | Product name. `$product.name` provides the product name in templates                                         |
| `n-handle`      | The last segment of product URL. `$!product.lastPathOfProductUrl()` provides the product handle in templates |
| `n-price`       | Product price. `$!product.price` provides the product price in templates                                     |
| `n-list-price`  | Product list price. `$!product.listPrice` provides the product list price in templates                       |
| `n-description` | Product description. `$!product.description` provides the product description in templates                   |

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
