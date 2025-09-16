# Features

{% hint style="warning" %}
You are reading the NextGen Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../../../../onsite-widgets/) here.

**Note: This feature is unique to NextGen widgets**
{% endhint %}

#### Features

***

Widgets can have various features that enhance their functionality. Some common features include:

* **tileSizeSettings** : Allows you to customize the size of each tile.
* **cssVariables** : Enables custom CSS variables for your widget.
* **handleLoadMore** : Determines whether more tiles should be loaded when the user scrolls to the end of the list.
* **limitTilesPerPage** : Limits the number of tiles displayed per page.

By default, many features are enabled, and they can be toggled off as required.

More features can be found here:

[**https://github.com/Stackla/widget-utils/blob/faa49c50d77a5b4554e80c5cef012c6e4e5ed3fc/src/widget-loader.ts#L22**](https://github.com/Stackla/widget-utils/blob/faa49c50d77a5b4554e80c5cef012c6e4e5ed3fc/src/widget-loader.ts#L22)

You can add these features/toggles by setting their corresponding properties in your widget settings. For example:

```
const myWidgetSettings = {
  features: {
    tileSizeSettings: {
      small: "173px",
      medium: "265.5px",
      large: "400px"
    },
    cssVariables: {
      "--my-mood-border": getMyMoodBorder()
    }
  }
};

loadWidget(myWidgetSettings);
```
