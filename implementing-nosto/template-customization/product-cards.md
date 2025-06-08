# Product Cards

Nosto recommends using shop-provided resources for rendering product cards in Nosto templates. Doing so offers:

- Faster onboarding
- Easier maintenance
- Consistent styling and behavior

Below are the recommended approaches.

## Custom Web Components

If your shop themes use web components, we suggest leveraging them in your Nosto templates as well. This avoids duplicating markup and logic between your shop and Nosto templates. For building web components efficiently, consider using [Lit](https://lit.dev/) or similar high-level frameworks.

## Nosto Web Components

Nosto offers several web components designed to simplify product card integration:

- **NostoDynamicCard**  
    Renders product cards entirely on the Shopify side.  
    *Requires alternate product card templates to be available within Shopify themes.*
    Choose this approach if the shop has existing product card markup in liquid templates that should be reused in Nosto campaign rendering.

- **NostoProductCard**  
    Provides a platform-agnostic custom element that delegates rendering to shop side Vue templates.
    Choose this approach for non-Shopify merchants that wish to maintain the product card templates in Shop side templates instead of Nosto's own templates.

- **NostoProduct**  
    Enhances static product card markup with interactive features such as:  
    - Swatch selection
    - Add-to-cart interactions  
    - Dynamic product image updates based on swatch and SKU selections

Nosto's web component offering is documented [here](../../apis/frontend/web-components)

## Style Reuse

If web components aren’t an option, we advise duplicating only the markup for product cards within Nosto templates while applying shop-side CSS rules to maintain consistent styling.
