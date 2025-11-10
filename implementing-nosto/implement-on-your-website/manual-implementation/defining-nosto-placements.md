# Defining Nosto placements

You can define placements for Nosto to use in two ways:

1. **Traditional approach**: Using native HTML elements with `nosto_element` class
2. **Web component approach**: Using the `<nosto-campaign>` web component

Both approaches mark locations in your site where Nosto can hook into and expose onsite content.

## Traditional div-element approach

You can define placements via native HTML elements with the `nosto_element` class. Each element marks a location in your site where Nosto can hook into and expose onsite content.

Here is an example of a `<div>` tag on the site:

```markup
<div class="nosto_element" id="frontpage-nosto-1" translate="no"></div>
```

The class needs to always reference `nosto_element` so Nosto understands that this element is available for onsite content placement. However the id `frontpage-nosto-1` is flexible but requires that each unique element-id has a matching placement defined in Nosto's admin dashboard in order to expose campaigns.

Here is an example of a page with multiple `<div>` elements:

```markup
// Three separate <divs> after another on a page

<div class="nosto_element" id="frontpage-nosto-1" translate="no"></div>
<div class="nosto_element" id="frontpage-nosto-2" translate="no"></div>
<div class="nosto_element" id="frontpage-nosto-3" translate="no"></div>

// You can also add the class and id to an element you are already using for other purposes

<div class="sidebar nosto_element" id="nosto-sidebar" translate="no">

   <h1>Hello World!</h1>
   <h2>This is the sidebar</h2>

   // You can also nest nosto elements within other wrapper elements

   <div class="nosto_element" id="nosto-sidebar-nested-1"></div>

</div>
```

## Web component approach with `<nosto-campaign>`

As an alternative to `nosto_element` divs, you can use the `<nosto-campaign>` web component. This approach provides cleaner markup integration.

### Basic usage

```html
<nosto-campaign placement="frontpage-nosto-1"></nosto-campaign>
<nosto-campaign placement="frontpage-nosto-2"></nosto-campaign>
<nosto-campaign placement="frontpage-nosto-3"></nosto-campaign>
```

or alternatively

```html
<nosto-campaign id="frontpage-nosto-1"></nosto-campaign>
<nosto-campaign id="frontpage-nosto-2"></nosto-campaign>
<nosto-campaign id="frontpage-nosto-3"></nosto-campaign>
```

for better compatibility with the scoped styling conventions of Velocity templates

### Advanced features

The web component supports additional features like lazy loading, product-specific recommendations, cart synchronization, and embedded Vue templates:

```html
<!-- Lazy-loaded campaign -->
<nosto-campaign placement="below-fold-recommendations" lazy></nosto-campaign>

<!-- Product-specific recommendations -->
<nosto-campaign placement="related-products" product-id="123456"></nosto-campaign>

<!-- Cart-synchronized campaign -->
<nosto-campaign placement="cart-recommendations" cart-synced></nosto-campaign>

<!-- Campaign with embedded Vue template -->
<nosto-campaign placement="best-sellers">
  <template>
    <div class="product-card" v-for="product in products">
      <span class="product-name">{{ product.name }}</span>
      <span class="product-price">{{ product.price }}</span>
    </div>  
  </template>
</nosto-campaign>
```

## When to choose each approach

**Use `nosto_element` divs when:**
- Working with existing legacy implementations
- You prefer traditional HTML markup patterns

**Use `<nosto-campaign>` web component when:**
- You need advanced features like lazy loading or cart synchronization
- You want better integration with modern web development practices
- You need embedded Vue templates for store-side templating
- Working with Shopify themes (additional integration patterns available)

## Setup and integration

To use the `<nosto-campaign>` web component, you need to include the Nosto Web Components library. For detailed setup instructions, see the [Web Components documentation](/apis/frontend/web-components/README.md).

