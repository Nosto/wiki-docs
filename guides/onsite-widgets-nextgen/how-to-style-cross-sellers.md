# Styling cross-sellers on Grid and Carousel Widgets

## Overview

When using Nosto Product Recommendations as cross-sellers in UGC widgets, you combine the power of social proof with recommendations to drive your conversion rate by increasing relevance and boost average order value.

This guide will explore how to style the layout of how cross-sellers are displayed in widgets.

## Getting Started

### Tag Content with Tags

Start by going to the Curate view, apply shopspots to 4 or 5 tiles and publish those tiles.

### Create a Filter

Creating a Filter in Nosto's UGC is really simple as outlined in [this Knowledgebase article](https://help.nosto.com/en/articles/5793303-how-do-i-create-a-filter). Assuming that we would like to only showcase content with products, select the following options:

* Name it "Cross-Sellers"
* Set it to sort by "Latest"
* Select the filter "Shopspots" and choose the "Show Shopspots" option

The rest of the options can be left to default.

### Create a Widget

Create a Grid or Carousel Widget and set its filter to "Cross-Sellers". Save and observe the preview with your content.

### Update the Widget Expanded Tile CSS

Open the Custom Code editor and click Expanded Tile. Here some examples of cross-sellers styling that you can customise:

#### Color of the ribbon (e.g. Grey):

```
.stacklapopup-products-item-image-recommendation-label {
```

#### Position of the ribbon (e.g. at the bottom):

```
.stacklapopup-products-item-image-recommendation-label {
```

#### Color and Font of the Cross-Seller label (e.g. Color "white", Font "Lato, sans-serif)

```
.stacklapopup-products-item-image-recommendation-label:after {
color: white;
font: 400 10px/18px "Lato, sans-serif";
}
```

#### Color, Font of the See Recommendations label (e.g. Color "#2C2C2C", Font "Lato, sans-serif)

```
.stacklapopup-products-amount-over-3:after {
color: #2C2C2C;
font: 400 10px/18px "Lato, sans-serif";
}
```
