# How to use the SDK

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

**Note: This feature is exclusive to NextGen widgets.**
{% endhint %}

The SDK can be utilised to power your widget with advanced functionality.

The SDK gives developers access to:

1. Their widget placement, along with all the parameters associated with it
2. The tiles service, allowing the manipulation of the tiles visible, and hidden.
3. The events service, allowing particular events to be hooked into, or emitted.

While the SDK is documented in the Understanding the SDK page, here is an example of how it has been utilised to build search functionality in the blank canvas widget template.

```
import { ISdk, loadWidget } from "@stackla/widget-utils"
import { config } from "./config"

declare const sdk: ISdk

loadWidget(sdk, {
  config: {
    ...config
  }
})

sdk.querySelector("#search-input")?.addEventListener("input", async e => {
  const eventTarget = e.target
  if (eventTarget instanceof HTMLInputElement) {
    const searchValue = eventTarget.value
    await sdk.tiles.searchTiles(searchValue)
  }
})
```

As soon as the widget is loaded, I perform a `querySelection` on an item in the placement using the SDK.

I attach an event, so that every time the input is modified, it performs a live search of the selection of tiles.

Note: Here I am accessing the Tiles Service via `sdk.tiles` and there is a plethora of methods that can be utilised to manipulate the tile view.

Now, every time a user searches, the tiles update to their search results.

Under the hood, this is how the tile search method is implemented

```
  public async searchTiles(query: string, clearExistingTiles: boolean = true) {
    this.widgetRequest.search = query

    if (clearExistingTiles) {
      const tiles = this.getPlacement().querySelector(".ugc-tiles")
      if (!tiles) {
        return
      }

      this.tiles = {}
      tiles.innerHTML = ""
    }

    return this.fetchTiles()
  }
```

This same functionality can take place in your code, without the use of the `searchTiles` method here, by utilising the suite of methods in the Tile Service.
