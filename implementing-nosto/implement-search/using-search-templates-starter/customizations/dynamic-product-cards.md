# Dynamic product cards

**For Shopify stores**, dynamic cards offer a powerful way to display products. Instead of building product cards from individual pieces of data (like a product's name and price), you can load the complete, ready-made product card directly from your theme.

This is the recommended approach when:

* Your product cards have complex logic, such as variant selectors, color swatches, or add-to-cart forms.
* You want to maintain a single source of truth for your product card templates within your Shopify theme.
* You need to display rich product information that may not be available as individual fields in the search API response.

### How It Works

The starter template uses a decorator to extract a product's handle from its URL. This handle is then passed as a prop to the `DynamicCard` Preact component, which in turn sets the `handle` attribute on the `nosto-dynamic-card` web component.

The key props you will work with are:

* **`handle`**: This is derived automatically by the `handleDecorator` from the `product.url`. You typically do not need to manage this manually.
* **`section`**: The ID of the section to render from your product template.
* **`template`**: The name of the alternate template to use for rendering.

> **Note:** To render a dynamic card, you must provide the `handle` prop along with either the `section` or `template` prop.

The `nosto-dynamic-card` web component leverages Shopify's built-in support for these features. When you provide a `section` or `template` prop, the component constructs the appropriate URL to fetch the pre-rendered HTML from your Shopify store.

> **Note:** The `template` prop usage is currently being deprecated and we recommend the `section` prop usage as it is recommended by Shopify and proven to be more robust.

Read more about the [Dynamic Product Cards web component](https://docs.nosto.com/techdocs/sdks/web-components/dynamic-product-cards).

### Configuration

#### 1. Configure the `DynamicCardProduct` Component

The template provides `DynamicCardProduct` components for both the search results page (SERP) and the autocomplete dropdown. You need to ensure the `template` prop matches a template available in your backend.

* **SERP Location:** `src/components/Product/DynamicCardProduct.tsx`
* **Autocomplete Location:** `src/components/Autocomplete/DynamicCardProduct/DynamicCardProduct.tsx`

For example, to change the template name for the search results page:

```tsx
// src/components/Product/DynamicCardProduct.tsx
import { DynamicCard, SerpElement } from "@/elements";
import type { Product } from "@nosto/search-js-sdk";

export default function DynamicCardProduct({ product }: { product: Product }) {
  return (
    <SerpElement product={product}>
      {/* Change this template name to match your backend template */}
      <DynamicCard handle={product.handle!} template="your-product-card-template" />
    </SerpElement>
  );
}
```

#### 2. Enable Dynamic Rendering in the Product Grid

The search results page (`Products.tsx`) can be configured to use either the static `Product.tsx` component (client-side rendering) or the `DynamicCardProduct.tsx` component (server-side rendering).

To use dynamic cards, ensure the component is imported from `DynamicCardProduct.tsx`.

```tsx
// src/components/Products/Products.tsx

// For dynamic, server-rendered cards:
import Product from "@/components/Product/DynamicCardProduct";

// For static, client-rendered cards, you would use:
// import Product from "@/components/Product/Product";

// ... rest of the component
```

### Visually Testing Dynamic Product Cards

When using dynamic product cards, the HTML for the cards is fetched directly from the merchant's website. This means that the styling for these cards is not included in the starter template's CSS.

To visually test the dynamic product cards in your local development environment, you need to include your store's CSS. You can do this by adding a `<link>` tag to the `<head>` of the `index.html` file in the root of the project.

For example:

```html
<!-- index.html -->
<!doctype html>
<html lang="en">
  <head>
    <!-- Add a link to your store's CSS file -->
    <link rel="stylesheet" href="https://your-store.com/store.css" />
    ...
  </head>
</html>
```

Replace `https://your-store.com/store.css` with the actual URL to your store's main stylesheet. This will allow you to see the correctly styled product cards when running the development server.
