# Event Listeners

{% hint style="warning" %}
You are reading the **NextGen Documentation**

**NextGen widgets** are a new and improved way to display UGC content onsite.&#x20;

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is a **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../../../../onsite-widgets/) here.

**Note: This feature is unique to NextGen widgets**
{% endhint %}

#### Events

***

Widgets support various events that can be triggered during different stages of their lifecycle. Here are some common events:

* **onLoad** : Triggered when the widget is fully loaded.
* **onTilesUpdated** : Triggered when the tile data changes.
* **onResize** : Triggered when the user resizes the widget.
* **onLoadMore** : Triggered when more tiles need to be loaded.
* **onTileExpand** : Triggered when a tile is expanded or collapsed.
* **onTileClose** : Triggered when a tile is closed.

Events are frequently updated, and can be found here:\
[https://github.com/Stackla/widget-utils/blob/3a7e9585700ea8b556ff266c3d69fa1aaff4eb84/src/events/index.ts](https://github.com/Stackla/widget-utils/blob/3a7e9585700ea8b556ff266c3d69fa1aaff4eb84/src/events/index.ts)

You can attach custom callback functions to these events using the `callbacks` property in your widget settings. For example:

```
const myWidgetSettings = {
  callbacks: {
    onLoad: () => console.log('Widget loaded!'),
    onTilesUpdated: () => console.log('Tile data updated!')
  }
};

loadWidget(myWidgetSettings);
```
