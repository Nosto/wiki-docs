# How to open and close expanded tiles

## How to open and close expanded tiles <a href="#how-to-open-and-close-expanded-tiles" id="how-to-open-and-close-expanded-tiles"></a>

Expanded tiles are dialog overlays contained within the core widget. They are used to display additional information about a tile, such as the tile's image, title, and description. This guide will show you how to open and close expanded tiles using the SDK.

### Opening an expanded tile <a href="#opening-an-expanded-tile" id="opening-an-expanded-tile"></a>

You can utilise sdk.openExpandedTile() to open an expanded tile. This method accepts a tileId as a parameter, which is the unique identifier of the tile you wish to expand.

```
sdk.openExpandedTile(tileId)
Copy
```

### Closing an expanded tile <a href="#closing-an-expanded-tile" id="closing-an-expanded-tile"></a>

You can utilise sdk.closeExpandedTile() to close an expanded tile. This method does not require any parameters.

```
sdk.closeExpandedTile()
Copy
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
