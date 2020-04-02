In order to move recommendations, you will need to edit your theme. Follow BigCommerce's instructions on [editing theme files](https://support.bigcommerce.com/s/article/Stencil-Themes#edit) to edit your theme.

Use BigCommerce's theme editor to find the associated Stencil file to edit it. In the editor, you will need to find the corresponding placeholder, after which, you can reposition, remove or even add a new placeholder.

Please note that we replace the content of the placement with the fully rendered recommendation, so please place an empty div on your theme where the recommendation should be placed. For example:
```
<div class="nosto_element" id="frontpage-nosto-1"></div>
```

You can read a more detailed explanation about how placements work in our [placements](https://help.nosto.com/en/articles/1883767-placements-general-article) help article.

> **Note**: We recommend duplicating your published theme i.e. creating a clone of the live theme, making edits on the duplicated theme and setting it as your published theme once you have validated your changes.

## List of default recommendation placeholders

Nosto places the following recommendation placeholders on the site.

| Element Name                                              | Theme File                          |
|-----------------------------------------------------------|-------------------------------------|
| `<div class="nosto_element" id="frontpage-nosto-1" />`    | templates/pages/home.html           |
| `<div class="nosto_element" id="frontpage-nosto-2" />`    | templates/pages/home.html           |
| `<div class="nosto_element" id="frontpage-nosto-3" />`    | templates/pages/home.html           |
| `<div class="nosto_element" id="frontpage-nosto-4" />`    | templates/pages/home.html           |
| `<div class="nosto_element" id="productpage-nosto-1" />`  | templates/pages/product.html        |
| `<div class="nosto_element" id="productpage-nosto-2" />`  | templates/pages/product.html        |
| `<div class="nosto_element" id="productpage-nosto-3" />`  | templates/pages/product.html        |
| `<div class="nosto_element" id="categorypage-nosto-1" />` | templates/pages/category.html       |
| `<div class="nosto_element" id="categorypage-nosto-2" />` | templates/pages/category.html       |
| `<div class="nosto_element" id="searchpage-nosto-1" />`   | templates/pages/search.html         |
| `<div class="nosto_element" id="searchpage-nosto-2" />`   | templates/pages/search.html         |
| `<div class="nosto_element" id="notfound-nosto-1" />`     | templates/pages/errors/generic.html |
| `<div class="nosto_element" id="notfound-nosto-2" />`     | templates/pages/errors/generic.html |
| `<div class="nosto_element" id="notfound-nosto-3" />`     | templates/pages/errors/generic.html |
| `<div class="nosto_element" id="cartpage-nosto-1" />`     | templates/pages/cart.html           |
| `<div class="nosto_element" id="cartpage-nosto-2" />`     | templates/pages/cart.html           |
| `<div class="nosto_element" id="cartpage-nosto-3" />`     | templates/pages/cart.html           |