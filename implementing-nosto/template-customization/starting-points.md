# Starting points

Each new Nosto account comes with three base recommendation templates to customize.

## Default

The `Default` template has the following features:

* Recommended products in a grid
* Alternate image on hover
* Ribbons for new, most viewed, and top-selling products
* Highlighting of discounts
* Add to cart functionality

## Carousel

The `Carousel` template extends the base template with a `Swiper`-based carousel to cycle between the recommended products.\
The library dependency is loaded via a script module, but a locally available version of the library can be used as well.

* Carousel implementation via Swiper
* Swiper is loaded via CDN URL into script module scope
* Swiper default styles are injected into the DOM
* Navigation module is loaded, and navigation buttons are provided in the DOM
* Basic mobile breakpoints are provided

## Swatches

The `Swatches` template extends the base template with SKU selection-aware product cards with swatches for color and size selection.

* Color and size swatch rendering
* `Product` and `SkuOptions` web components to maintain swatch selection state and abstract the add-to-cart logic away
* Web components library is loaded via CDN URL into script module scope
* Product image is updated based on color swatch selection
* Add to cart button becomes visible when color and size values have been chosen
