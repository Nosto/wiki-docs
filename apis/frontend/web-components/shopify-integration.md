# Shopify Integration

This page lists proven integration patterns and code snippets for the Shopify specific web components such as `DynamicCard`. Since integrations depend a lot based on theme structure the solutions are grouped by Theme name.

## `DynamicCard` Integration

### Dawn theme

The following snippet works as the product card template (e.g. `templates/product.card.liquid`)

```liquid
{% layout none %}
{% render 'card-product', card_product: product %}
```

### Horizon theme

The following snippet works as the product card template (e.g. `templates/product.card.liquid`)

```liquid
{% layout none %}

{% liquid
  # Create the children content manually since we can't use the block system in templates
  capture children
    # Render the card gallery
    render 'card-gallery', closest.product: product, block: 'gallery'
    
    # Render the group with title, price, and swatches
    echo '<div class="layout-panel-flex layout-panel-flex--column gap-style" style="--gap: 4px;">'
    echo '<h3 class="h5">' | append: product.title | append: '</h3>'
    render 'price', product_resource: product
    render 'variant-swatches', product_resource: product
    echo '</div>'
  endcapture
%}

{% render 'product-card', product: product, children: children %}
```