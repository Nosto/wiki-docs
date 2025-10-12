# Defining Nosto placements

You can define placements for Nosto to use in two ways:

1. **Traditional approach**: Using div-elements with `nosto_element` class
2. **Web component approach**: Using the `<nosto-campaign>` web component

Both approaches mark locations in your site where Nosto can hook into and expose onsite content.

## Traditional div-element approach

You can define placements via a code block called div-elements. Each element marks a location in your site where Nosto can hook into and expose onsite content.

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

As an alternative to `nosto_element` divs, you can use the `<nosto-campaign>` web component. This approach exposes campaigns without adding wrapper divs, providing cleaner markup integration.

### Basic usage

```html
<nosto-campaign placement="frontpage-nosto-1"></nosto-campaign>
<nosto-campaign placement="frontpage-nosto-2"></nosto-campaign>
<nosto-campaign placement="frontpage-nosto-3"></nosto-campaign>
```

### Advanced features

The web component supports additional features like lazy loading, product-specific recommendations, and cart synchronization:

```html
<!-- Lazy-loaded campaign -->
<nosto-campaign placement="below-fold-recommendations" lazy></nosto-campaign>

<!-- Product-specific recommendations -->
<nosto-campaign placement="related-products" product-id="123456"></nosto-campaign>

<!-- Cart-synchronized campaign -->
<nosto-campaign placement="cart-recommendations" cart-synced></nosto-campaign>
```

## When to choose each approach

**Use `nosto_element` divs when:**
- You need wrapper divs for styling or layout purposes
- Working with existing legacy implementations
- You prefer traditional HTML markup patterns

**Use `<nosto-campaign>` web component when:**
- You want cleaner markup without wrapper divs
- You need advanced features like lazy loading or cart synchronization
- You want better integration with modern web development practices
- Working with Shopify themes (additional integration patterns available)

## Setup and integration

To use the `<nosto-campaign>` web component, you need to include the Nosto Web Components library. For detailed setup instructions and Shopify-specific integration patterns, see the [Web Components documentation](/apis/frontend/web-components/README.md) and [Shopify integration guide](/apis/frontend/web-components/shopify-integration.md).

