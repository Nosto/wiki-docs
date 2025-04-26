# Starting points

Each new Nosto account comes with three base recommendation templates to customize

## Default

The `Default` template has the following features

* recommended products in a grid
* alternate image on hover
* ribbons for new, most viewed and top selling products
* highlighting of discounts
* add to cart functionality

## Carousel

The `Carousel` template extends the base template with a `Swiper` based carousel to cycle between the recommended products.
The library dependency is loaded via a script module, but a locally available version of the library can be used as well

* Carousel implementation via Swiper
* Swiper is loaded via cdn url into script module scope
* Swiper default styles are injected into DOM
* Navigation module is loaded and navigation button provided in DOM
* Basic mobile break points are provided

## Swatches

The `Swatches` template extends the base template with SKU selection aware product cards with swatches for color and size selection.

* Color and size swatch rendering
* `NostoProduct` and `NostoSkuOptions` web components to maintain swatch selection state and abstract the add to cart logic away
* Web components library is loaded via cdn url into script module scope
* Product image is updated based on color swatch selection
* Add to cart button becomes visible when color and size values have been chosen

More info on the available Nosto web components is available [here](https://github.com/Nosto/web-components).
