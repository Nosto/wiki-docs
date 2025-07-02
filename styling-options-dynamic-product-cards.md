---
description: >-
  Dynamic Product Carts allow you to utilize your Shopify card markup directly
  in Nosto-powered experiences, including Product Recommendations, Dynamic
  Bundles, Category Merchandising & Search.
---

# Styling Options - Dynamic Product Cards

## What Dynamic Product Cards are

Our `NostoDynamicCard`  Web Component offers a clean, flexible way to render Shopify product cards within Nosto-powered experiences, by fully deferring product markup to Shopify. This is especially useful for merchants who want their storefront's visual identity and product logic to remain unified, whilte still utilising Nosto's Personalization capabilities.

This is the recommended method to use when you want the actual product card layout to be sourced from Shopify, not duplicated or maintained separately in Nosto templates.

## Why Use Dynamic Product Cards

Traditionally, rendering product cards within Nosto-powered experiences required replicating the card markup into Nosto templates. This created maintenance overhead and visual drift between Shopify themes and Nosto experiences.

The `NostoDynamicCard` solves this by:

* Rendering product cards using native Shopify snippets and templates
* Letting Shopify handle product logic (e.g., badges, swatches, sale labels)
* Keeping visual consistency across theme and personalization areas
* Reducing code duplication and template maintenance
* Automatically adapting your Nosto Experience with changed in Shopify

It works by passing a `handle` and optional `variant-id` into a Shopify template designed to render a single product card.

## How It Works

`NostoDynamicCard` is a Web Component that runs in the browser. It accepts a minimal set of attributes:

| **Attribute** | **Description**                                   |
| ------------- | ------------------------------------------------- |
| `handle`      | The Shopify product handle (required)             |
| `template`    | Name of the alternate product template (required) |
| `variant-id`  | Optional variant ID to preselect                  |

When this is rendered inside a Nosto template, Shopify is responsible for fetching and rendering the final product card markup using the specified alternate template.

## Implementation Steps (Example: Dawn Theme)

To give you full context, let's use an example of how this could be set up.&#x20;



1.  **Create an alternate template**

    In your Shopify theme, create a new product template (e.g. `templates/product.card.liquid`) with the following content:

    ```
    {% layout none %}
    {% render 'card-product', card_product: product %}
    ```
2.  **Ensure the card snippet exists**

    The snippet `card-product` is commonly used in themes like Dawn. If your theme uses a different one, adjust accordingly.
3.  **Enable Web Components in Nosto**

    In the Nosto admin, go to **Settings > Recommendations** and enable **Web Components**.
4.  **Use in Nosto Template**

    In your Nosto template (e.g., for recommendations), use the component like this:

    ```
    #foreach($product in $products)
    <nosto-dynamic-card handle="$!product.handle" template="card">
      <div class="product-card-skeleton"></div>
    </nosto-dynamic-card>
    #end
    ```

## Where to Use It

`NostoDynamicCard` is compatible with any Nosto module that displays products:

* **Product Recommendations**
* **Dynamic Bundles**
* **Personalized Search**
* **Category Merchandising (Universal)**

This ensures that across carousels, grids, and dynamic lists, your product cards always match Shopify's theme logic - no duplication needed.

## Best Practices

* Keep the alternate template minimal. Don’t include layout wrappers or page-level HTML, but only  the product card snippet.
* Always test your template in a preview to ensure it works with real handles.
* Consider adding skeleton loaders or fallback content inside the element for smoother rendering.
* Use consistent naming across theme and Nosto templates (e.g., `template="card"` maps to `product.card.liquid`).

## Other Web Components

If you're interested for other options, refer also to our [Web Components Overview](https://docs.nosto.com/techdocs/apis/frontend/oss/web-components/loading-web-components).

## Summary

`NostoDynamicCard` is the best choice when your product card rendering should be entirely owned by Shopify. It removes duplication, aligns your storefront visuals, and makes maintaining personalization layouts much easier across Nosto's modules.

Of course, in case you intend to have specific (or all) Nosto Experiences appear differently, we still offer fully customized templates.&#x20;
