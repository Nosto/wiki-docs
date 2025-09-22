# How to make expanded tiles vertical

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../onsite-widgets/) here.
{% endhint %}

## How to make the expanded tile vertical <a href="#how-to-make-the-expanded-tile-vertical" id="how-to-make-the-expanded-tile-vertical"></a>

1. Remove Unnecessary Classes In the ExpandedTiles template `expanded-tile.template.tsx`, remove the following class names to prevent horizontal swiper behavior:

* `swiper-wrapper`
* `swiper`
* `swiper-expanded`
* `swiper-slide`

2. Add this click listener to close Tile when clicking outside of Expanded Tile in vertical list in `widget.tsx`. To close the Expanded Tile when clicking outside of it, add this JavaScript event listener:

```
const dialog = sdk.querySelector("#overlay-expanded-tiles")
dialog.addEventListener("click", event => {
  if (dialog === event.target) {
    sdk.closeExpandedTiles()
  }
})
```

3. Hide navigation arrows on Expanded Tile: Since the tile is now vertical, hide the left/right arrows using CSS.

```
#nosto-ugc-extensions {
    #overlay-expanded-tiles {
        overflow-x: hidden;
        overflow-y: auto;
        -webkit-overflow-scrolling: touch;
    }

    .swiper-expanded-button-prev,
    .swiper-expanded-button-next {
        display: none !important;
    }
}

@media only screen and (max-width: 992px) {
    .ugc-tile {
        width: 100vw;
    }
    .panel,
    .panel-left,
    .panel-right {
        width: 100vw;
        box-sizing: border-box;
    }
}
Copy
```

## Summary <a href="#summary" id="summary"></a>

✔ Expanded tile will be displayed vertically.&#x20;

✔ Clicking outside the tile will close it.&#x20;

✔ Navigation arrows are hidden.&#x20;

✔ Vertical scrolling is enabled for long content.
