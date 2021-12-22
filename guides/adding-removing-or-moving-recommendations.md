# Adding/Moving Recommendations

In order to move recommendations, you will need to edit your theme. Follow Shopify's instructions on [editing theme code](https://shopify.dev/themes/getting-started/customize) to edit your theme.

Use Shopify's theme editor to find the associated Liquid file to edit it. In the editor, you will need to find the corresponding placeholder, after which, you can reposition, remove or even add a new placeholder.

> **Note**: We recommend [duplicating your published theme](https://help.shopify.com/manual/using-themes/theme-files#duplicate-themes) i.e. creating a clone of the live theme, making edits on the duplicated theme and setting it as your published theme once you have validated your changes.

## List of default recommendation placeholders

Nosto places the following recommendation placeholders on the site.

| Element Name | Theme File (OS1) | Theme File (OS2) - Updated | Theme File (OS2) - Added |
| :--: | :--: | :--: | :--: |
| `<div class="nosto_element" id="frontpage-nosto-1" />` | templates/index.liquid | templates/index.json | section/nosto-index.liquid |
| `<div class="nosto_element" id="frontpage-nosto-2" />` | templates/index.liquid | templates/index.json | section/nosto-index.liquid |
| `<div class="nosto_element" id="frontpage-nosto-3" />` | templates/index.liquid | templates/index.json | section/nosto-index.liquid |
| `<div class="nosto_element" id="frontpage-nosto-4" />` | templates/index.liquid | templates/index.json | section/nosto-index.liquid |
| `<div class="nosto_element" id="productpage-nosto-1" />` | templates/product.liquid | templates/product.json | section/nosto-product.liquid |
| `<div class="nosto_element" id="productpage-nosto-2" />` | templates/product.liquid | templates/product.json | section/nosto-product.liquid |
| `<div class="nosto_element" id="productpage-nosto-3" />` | templates/product.liquid | templates/product.json | section/nosto-product.liquid |
| `<div class="nosto_element" id="categorypage-nosto-1" />` | templates/collection.liquid | templates/collection.json | section/nosto-collection-prepend.liquid |
| `<div class="nosto_element" id="categorypage-nosto-2" />` | templates/collection.liquid | templates/collection.json | section/nosto-collection-append.liquid |
| `<div class="nosto_element" id="searchpage-nosto-1" />` | templates/search.liquid | templates/search.json | section/nosto-search-prepend.liquid |
| `<div class="nosto_element" id="searchpage-nosto-2" />` | templates/search.liquid | templates/search.json | section/nosto-search-append.liquid |
| `<div class="nosto_element" id="notfound-nosto-1" />` | templates/404.liquid | templates/404.json | section/nosto-404.liquid |
| `<div class="nosto_element" id="notfound-nosto-2" />` | templates/404.liquid | templates/404.json | section/nosto-404.liquid |
| `<div class="nosto_element" id="notfound-nosto-3" />` | templates/404.liquid | templates/404.json | section/nosto-404.liquid |
| `<div class="nosto_element" id="cartpage-nosto-1" />` | templates/cart.liquid | templates/cart.json | section/nosto-cart.liquid |
| `<div class="nosto_element" id="cartpage-nosto-2" />` | templates/cart.liquid | templates/cart.json | section/nosto-cart.liquid |
| `<div class="nosto_element" id="cartpage-nosto-3" />` | templates/cart.liquid | templates/cart.json | section/nosto-cart.liquid |