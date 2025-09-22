# Open and close expanded tiles

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/) here.

**Note: This feature is exclusive to NextGen widgets.**
{% endhint %}

## How to open and close expanded tiles <a href="#how-to-open-and-close-expanded-tiles" id="how-to-open-and-close-expanded-tiles"></a>

Expanded tiles are dialog overlays contained within the core widget. They are used to display additional information about a tile, such as the tile's image, title, and description. This guide will show you how to open and close expanded tiles using the SDK.

### Opening an expanded tile <a href="#opening-an-expanded-tile" id="opening-an-expanded-tile"></a>

You can utilise sdk.openExpandedTile() to open an expanded tile. This method accepts a tileId as a parameter, which is the unique identifier of the tile you wish to expand.

```
sdk.openExpandedTile(tileId)
```

### Closing an expanded tile <a href="#closing-an-expanded-tile" id="closing-an-expanded-tile"></a>

You can utilise sdk.closeExpandedTile() to close an expanded tile. This method does not require any parameters.

```
sdk.closeExpandedTile()
```

### Example <a href="#example" id="example"></a>

Here is an example of how you can open and close an expanded tile using the SDK.

```
import { ISdk } from "@stackla/widget-utils"

declare const sdk: ISdk

sdk.querySelectorAll(".tile").forEach(tile => {
  tile.addEventListener("click", () => {
    const tileId = tile.getAttribute("data-tile-id")
    sdk.openExpandedTile(tileId)
  })
})

setTimeout(() => {
  sdk.closeExpandedTile()
}, 5000)
```
