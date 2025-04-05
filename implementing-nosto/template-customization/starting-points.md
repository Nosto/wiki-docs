# Starting points

Each new Nosto account comes with three base recommendation templates to customize

## Default

The base template has the following features
* recommended products in a grid
* alternate image on hover
* ribbons for new, most viewed and top selling products
* highlighting of discounts
* add to cart functionality

## Swiper

The `Swiper` template extends the base template with a `Swiper` based carousel to cycle between the recommended products.
The library dependency is loaded via a script module, but a locally available version of the library can be used as well

## SKUs

The SKUs template should be applied in cases where product variant data should be incorporated in the product cards.
With it's default configuration this template works best for fashion products by breaking the variant into two dimensions:
* colors - shown in the bottom of the product card
* sizes - show as an overlay on top of the image

The size elements act as add to cart buttons and will add the chosen combination of color and size to the cart