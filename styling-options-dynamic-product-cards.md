---
description: >-
  Dynamic Product Carts allow you to utilize your Shopify card markup directly
  in Nosto-powered experiences, including Product Recommendations, Dynamic
  Bundles, Category Merchandising & Search.
---

# Styling Options - Dynamic Product Cards

## What Dynamic Product Cards are

Our `DynamicCard` Web Component offers a clean, flexible way to render Shopify product cards within Nosto-powered experiences, by fully deferring product markup to Shopify. This is especially useful for merchants who want their storefront's visual identity and product logic to remain unified, whilte still utilising Nosto's Personalization capabilities.

This is the recommended method to use when you want the actual product card layout to be sourced from Shopify, not duplicated or maintained separately in Nosto templates.

## Why Use Dynamic Product Cards

Traditionally, rendering product cards within Nosto-powered experiences required replicating the card markup into Nosto templates. This created maintenance overhead and visual drift between Shopify themes and Nosto experiences.

The `DynamicCard` solves this by:

* Rendering product cards using native Shopify snippets and templates
* Letting Shopify handle product logic (e.g., badges, swatches, sale labels)
* Keeping visual consistency across theme and personalization areas
* Reducing code duplication and template maintenance
* Automatically adapting your Nosto Experience with changed in Shopify

It works by passing a `handle` and optional `variant-id` into a Shopify template designed to render a single product card.

## How It Works

`DynamicCard` is a Web Component that runs in the browser. It accepts a minimal set of attributes:

| **Attribute** | **Description**                                     |
| ------------- | --------------------------------------------------- |
| `handle`      | The Shopify product handle (required)               |
| `section`     | Name of the section to use                          |
| `template`    | Name of the alternate product template (deprecated) |
| `variant-id`  | Optional variant ID to preselect                    |

When this is rendered inside a Nosto template, Shopify is responsible for fetching and rendering the final product card markup using the specified alternate template.

## Implementation Steps (Example: Dawn Theme)

To give you full context, let's use an example of how this could be set up.

1.  **Identify the product grid section of the collection template**

    Search for the type of the product grid section in `templates/collection.{json|liquid}`

    ```
    {
      "sections": {
        "banner": {
          "type": "main-collection-banner",
          "settings": {
            ...
          }
        },
        "product-grid": {
          "type": "main-collection-product-grid",
          ...
        }
      }
    }      
    ```

    The type is `main-collection-product-grid` in the Dawn theme
2.  **Identify the product card snippet in the product grid section**

    Open the file for the section found in the previous step, `sections/main-collection-product-grid.liquid` in Dawn. Search for the snippet usage that renders the product card:

    ```
    {% render 'card-product',
      card_product: product,
      media_aspect_ratio: section.settings.image_ratio,
      image_shape: section.settings.image_shape,
      show_secondary_image: section.settings.show_secondary_image,
      show_vendor: section.settings.show_vendor,
      show_rating: section.settings.show_rating,
      lazy_load: lazy_load,
      skip_styles: skip_card_product_styles,
      quick_add: section.settings.quick_add,
      section_id: section.id
    %}
    ```

    The correct snippet is `card-product` in the Dawn theme
3.  **Create a product card section**

    Create a new product card section (e.g. `sections/product-card.liquid`) with the following content:

    ```
    {% render 'card-product', card_product: product %}

    {% schema %}
    {
      "name": "ProductCard Section",
      "presets": [
        {
          "name": "Product Card Section",
          "category": "Personalization"
        }
      ]
    }
    {% endschema %}
    ```

    Replace `card-product` and the parameters with the snippet usage you found in the previous step. The parameters for the snippet will need to be replaced with the relevant section settings.
4.  **Enable Web Components in Nosto**

    In the Nosto admin, go to **Settings > Account Settings** and enable **Web Components**.
5.  **Use in Nosto Template**

    In your Nosto template (e.g., for recommendations), use the component like this:

    ```
    #foreach($product in $products)
    <nosto-dynamic-card handle="$!product.handle" section="product-card">
      <div class="product-card-skeleton"></div>
    </nosto-dynamic-card>
    #end
    ```

## Where to Use It

`DynamicCard` is compatible with any Nosto module that displays products:

* **Product Recommendations**
* **Dynamic Bundles**
* **Personalized Search**
* **Category Merchandising (Universal)**

This ensures that across carousels, grids, and dynamic lists, your product cards always match Shopify's theme logic - no duplication needed.

## Best Practices

* Keep the alternate template minimal. Don’t include layout wrappers or page-level HTML, but only the product card snippet.
* Always test your template in a preview to ensure it works with real handles.
* Consider adding skeleton loaders or fallback content inside the element for smoother rendering.
* Use consistent naming across theme and Nosto templates (e.g., `template="card"` maps to `product.card.liquid`).

## Other Web Components

If you're interested for other options, refer also to our [Web Components Overview](https://docs.nosto.com/techdocs/apis/frontend/oss/web-components/loading-web-components).

## Summary

`DynamicCard` is the best choice when your product card rendering should be entirely owned by Shopify. It removes duplication, aligns your storefront visuals, and makes maintaining personalization layouts much easier across Nosto's modules.

Of course, in case you intend to have specific (or all) Nosto Experiences appear differently, we still offer fully customized templates.
