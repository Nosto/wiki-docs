# Configuring your widget

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../../../../onsite-widgets/) here.

**Note: This feature is unique to NextGen widgets**
{% endhint %}

When using the blank canvas widget, we do not have configuration on the backend available for your use.

All configuration is now managed in the frontend by the developer.

You will find a config.ts available with the following data & documentation.

````typescript
```typescript
import { ExpandedTileOptions, InlineTileOptions, Style } from "packages/widget-utils/dist/esm"

export const config: {
  style: Style
  inlineTile: InlineTileOptions
  expandedTile: ExpandedTileOptions
} = {
  style: {
    // The background color of the widget, uses var(--widget-background) in css-variables.ts
    widget_background: "#ffffff",
    // The background color of the tile, uses var(--text-tile-background) in css-variables.ts
    text_tile_background: "#ffffff",
    // The font color of the tiles, uses var(--text-tile-font-color) in css-variables.ts
    text_tile_font_color: "#000000",
    // The font color of the user handle, uses var(--text-tile-user-handle-font-color) in css-variables.ts
    text_tile_user_handle_font_color: "#636062",
    // The font color of the user name, uses var(--text-tile-user-name-font-color) in css-variables.ts
    text_tile_user_name_font_color: "#ffffff",
    // The font size of the tile contents, uses var(--text-tile-font-size) in css-variables.ts
    text_tile_font_size: "10",
    // The font size of the user handle, uses var(--text-tile-user-handle-font-size) in css-variables.ts
    text_tile_user_handle_font_size: "14",
    // The font size of the user name, uses var(--text-tile-user-name-font-size) in css-variables.ts
    text_tile_user_name_font_size: "14",
    // The margin of the widget, and gap between tiles, uses var(--margin) in css-variables.ts
    margin: "10",
    // The mode of what happens when a tile is clicked, options: [EXPAND], [ORIGINAL_URL] or [CUSTOM]
    // [EXPAND] will expand the tile, [ORIGINAL_URL] will open the original URL associated with a social media image, [CUSTOM] will open a custom URL (not implemented)
    click_through_url: "[EXPAND]",
    // The background image of the icon, uses var(--shopspot-icon) in css-variables.ts, defaults to #000
    shopspot_icon: "",
    // Whether the tile should automatically pull new tiles or not
    auto_refresh: "true",
    // Whether the widget should only load x amount of tiles per page
    tiles_per_page: "",
    enable_custom_tiles_per_page: true,
    // Whether the widget should load more tiles on scroll, button or static
    load_more_type: "button",
    // The name of the widget
    name: "Blank Canvas",
    // The link color of the tile, uses var(--text-tile-link-color) in css-variables.ts
    text_tile_link_color: "",
    // The minimum amount of tiles required to show the widget
    minimal_tiles: "6",
    // Tile size: small, medium, large
    inline_tile_size: "medium",
    // The border radius of the inline tile, uses var(--inline-tile-border-radius) in css-variables.ts
    inline_tile_border_radius: "5",
    // The border radius of the expanded tile, uses var(--expanded-tile-border-radius) in css-variables.ts
    expanded_tile_border_radius: "5"
  },
  expandedTile: {
    // Whether to show the caption of the tile
    show_caption: true,
    // Whether to show the timestamp of the tile
    show_timestamp: true,
    // Whether to show the navigation options of the tile
    show_nav: true,
    // Whether to show the sharing options of the tile
    show_sharing: true,
    // Whether to show the shopspots of the tile
    show_shopspots: true,
    // Whether to show the products of the tile
    show_products: true,
    // Whether to show the tags of the tile
    show_tags: true,
    // Whether to show the votes of the tile
    show_votes: true,
    // Whether to show the cross sellers of the tile
    show_cross_sellers: true,
    // Whether to show the add to cart
    show_add_to_cart: true
  },
  inlineTile: {
    // Whether to show the navigation options of the tile
    show_nav: true,
    // Whether to show the sharing options of the tile
    show_sharing: true,
    // Whether to show the shopspots
    show_shopspots: true,
    // Whether to show the tags of the tile
    show_tags: true,
    // Whether to show the timestamp of the tile
    show_timestamp: true,
    // Whether to show the caption of the tile
    show_caption: true,
    // Whether to show the products of the tile
    show_products: true,
    // Whether to show the add to cart functionality
    show_add_to_cart: true,
    // Whether to auto play the video
    auto_play_video: false,
    // Whether to show the inline tiles
    show_inline_tiles: true,
    // Whether to show the carousel
    show_carousel: false
  }
}

```

````

