---
description: Onsite Widgets JavaScript API Reference
---

# API Reference for Content Widgets

{% hint style="warning" %}
You are reading the **Classic Widget Documentation**

Classic widgets are a previous widget version for onsite widgets created before September 23rd.

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../../../guides/widgets-nextgen/).

**Note: This feature is unique to Classic widgets**
{% endhint %}

* [Inline Tiles API](reference.md#inline-tiles)
  * [Events](reference.md#inline-tiles-events)
  * [Legacy Callbacks](reference.md#inline-tiles-callbacks)
  * [Translation Strings](reference.md#inline-tiles-translation)
* [Global Widget Events API](reference.md#parent-page)
  * [Events](reference.md#events)
  * [Methods](reference.md#methods)

## Inline Tiles API

### Events

The following events are available in Waterfall, Carousel, Gallery, Feed, Grid, Masonry, Nightfall, and all future widgets.

```
$(document).on('widget:ready', function (e, instance) {
    instance
        .on('init', function (e, instance) {
            console.log(e.type, instance);
        })
        .on('load', function (e, listData) {
            console.log(e.type, listData);
        })
        .on('beforeRender', function (e, $appendRoot, listData) {
            console.log(e.type, $appendRoot, listData);
        })
        .on('beforeIterate', function (e, tileData, tileId) {
            console.log(e.type, tileData, tileId);
        })
        .on('beforeAppend', function (e, $tile, tileData, $appendRoot) {
            console.log(e.type, $tile, tileData, $appendRoot);
        })
        .on('iterate', function (e, tileData, tileHtml) {
            console.log(e.type, $tile, tileData, $appendRoot);
        })
        .on('append', function (e, $tile, tileData, $appendRoot) {
            console.log(e.type, $tile, tileData, $appendRoot);
        })
        .on('render', function (e, $appendRoot, listData) {
            console.log(e.type, $appendRoot, listData);
        });
});
```

| Order | Event Name    | Description                                                                                                                                                                                                                                                                                                    | Arguments                                                                                                 |
| ----- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| #1    | init          | Triggered when the widget instance is available.                                                                                                                                                                                                                                                               | <ol><li>e (Object)</li><li>instance (Object)</li></ol>                                                    |
| #2    | load          | Triggered when the initial data is loaded.                                                                                                                                                                                                                                                                     | <ol><li>e (Object)</li><li>listData (Array)</li></ol>                                                     |
| #3    | beforeRender  | Triggered before widget starts to render.                                                                                                                                                                                                                                                                      | <ol><li>e (Object)</li><li>$appendRoot (jQuery)</li><li>listData (Array)</li></ol>                        |
| #4    | beforeIterate | <p>Triggered before a tile data is decorated and a tile HTML is generated.</p><p>You can update the tile data. If the widget supports for the Custom Template,<br>you can use your customized data in the template. The Custom Template feature is currently<br>available in the Grid and Masonry widgets.</p> | <ol><li>e (Object)</li><li>tileData (Array)</li><li>tileId (String)</li></ol>                             |
| #5    | beforeAppend  | <p>Triggered before a tile is appended to the container.</p><p>You can modify a tile element before it is being appeneded to the container.</p>                                                                                                                                                                | <ol><li>e (Object)</li><li>$tile (jQuery)</li><li>tileData (Array)</li><li>$appendRoot (jQuery)</li></ol> |
| #6    | iterate       | Triggered after a tile is decorated and the tile HTML is generated.                                                                                                                                                                                                                                            | <ol><li>e (Object)</li><li>$tile (jQuery)</li><li>tileData (Array)</li></ol>                              |
| #7    | append        | Triggered after a tile is appended to the container.                                                                                                                                                                                                                                                           | <ol><li>e (Object)</li><li>$tile (jQuery)</li><li>tileData (Array)</li><li>$appendRoot (jQuery)</li></ol> |
| #8    | render        | Triggered after the widget is rendered.                                                                                                                                                                                                                                                                        | <ol><li>e (Object)</li><li>$appendRoot (jQuery)</li><li>listData (Array)</li></ol>                        |
| #9    | loadMore      | Triggered after the widget load more data.                                                                                                                                                                                                                                                                     | <ol><li>e (Object)</li><li>$appendRoot (jQuery)</li><li>listData (Array)</li></ol>                        |

### Legacy Callbacks

This section is only for Waterfall and Carousel widgets.

```
Callbacks.prototype.onCompleteJsonToHtml = function ($tile, tileData) {
    return $tile; // required, don't remove
};
```

| Order | Callback Name         | Arguments                                                                                                                                                     | Must Return                                 | Description                                                                                                                                                                          |
| ----- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| #1    | onBeforeRenderIsotope | <ol><li>containerWidth (Number)</li><li>maxTileWidth (Number)</li><li>margin (Number)</li><li>numColumns (Number)</li><li>currentTileWidth (Number)</li></ol> | {tileWidth: (Number), numColumns: (Number)} | Invoked before widget rendering and window resizing. Use it when you need to change the widget layout. Note this callback is only available in Vertical Fluid and Waterfall widgets. |
| #2    | onBeforeRenderTiles   | <ol><li>addedIds (Array)</li><li>addedData (Object)</li></ol>                                                                                                 | –                                           | Invoked befored tiles are rendered. Use it when you want to add, remove, or update tiles before rendering.                                                                           |
| #3    | onCompleteJsonToHtml  | <ol><li>$tile (jQuery)</li><li>tileData (Object)</li></ol>                                                                                                    | $tile (jQuery)                              | Invoked before each tile is appended to container. Use it when you need to modify the tile HTML structure according to its data.                                                     |
| #4    | getTileWidth          | <ol><li>$tile (jQuery)</li><li>tileWidth (Number)</li><li>margin (Number)</li><li>numColumns (Number)</li></ol>                                               | tileWidth (Number)                          | Invoked before each tile dimension is set. Use it when you need to modify tile width.                                                                                                |
| #5    | onCalculateDimensions | <ol><li>$tile (jQuery)</li></ol>                                                                                                                              | –                                           | Invoked after each tile is appended to container. Use it when you need to resize inside elements according to actual tile dimension.                                                 |
| #6    | onCompleteAddingToDom | <ol><li>$tile (jQuery)</li></ol>                                                                                                                              | –                                           | Invoked after each tile is appended to container.                                                                                                                                    |
| #7    | onCompleteRenderImage | <ol><li>$image (jQuery)</li></ol>                                                                                                                             | –                                           | Invoked after each tile image is loaded.                                                                                                                                             |
| #8    | onCompleteRenderTiles | <ol><li>$addedTiles (jQuery)</li><li>addedData (Object)</li></ol>                                                                                             | –                                           | Invoked after tiles are rendered. Use it when you need to update tiles after they are rendered.                                                                                      |
|       | onScrollLoad          | <ol><li>$tiles (jQuery)</li><li>isLoading (Boolean)</li></ol>                                                                                                 | –                                           | Invoked when a widget loads more tiles by user scrolling. Use it when you need to update ‘Loading…’ text.                                                                            |
|       | onScrollEnd           | <ol><li>$tiles (jQuery)</li><li>isLoading (Boolean)</li></ol>                                                                                                 | –                                           | Invoked when a widget doesn’t have any tiles to load after user scrolling. Use it when you need to update ‘There is no more content to be displayed.’ text.                          |
|       | onMessage             | <ol><li>type (String)</li><li>data (Object)</li></ol>                                                                                                         | –                                           | Invoked when a widget receives customised message from parent page.                                                                                                                  |

### Translation Strings

You can modify or translate Widget texts by accessing `window.Strings` global variable. The following strings are only useful on the Waterfall, Grid, Masonry, Quadrant, Feed, and Nightfall widgets.

```
Strings.load_more = 'Load more content';
Strings.loading = 'Loading...';
Strings.scroll_end = 'There is no more content to be displayed.'; // Waterfall widget only
Strings.voting_message = 'A vote has already been registered from this IP address.';
```

## Global Widget Events API

The following Inline Tiles API are only available in Waterfall, Carousel, and Gallery widgets

You can access all the following events and methods via the `window.Stackla.WidgetManager`\
global variable.

### Events

You can track user interactions by subscribing to Widget events.

#### Example Usage (for both Event and Methods)

```
Stackla.WidgetManager.on('tileExpand', function (e, data) {
    // Maybe you need to filter when you have multiple widgets on the same page.
    if (data.widgetId === 9999) {
        // do something...
    }
});
```

Note: To avoid manually checking the existence of the `Stackla.WidgetManager` global variable\
we recommend you use the Expanded Tile Code Editor in our Nosto Admin Portal.

#### Available Events

beforeExpandedTileClose\
Triggered before the Expanded Tile closes

* e
* data.el
* data.tileData
* data.widgetId

| Event Name                                      | Description                                              | Callback Arguments                                                                                                                                                                                                                                                               |
| ----------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| load                                            | Triggered after the widget has been loaded               | <ul><li>e</li><li>data.filterId</li><li>data.hashId</li><li>data.styleName</li><li>data.uniqueId</li><li>data.widgetId</li><li>data.widgetType</li></ul>                                                                                                                         |
| tileExpand                                      | Triggered after the Tile opens                           | <ul><li>e</li><li><p>data.expandType</p><ul><li>‘expand’: Lightbox</li><li>‘stack’: Full Stack</li><li>’tile’: Expanded Tile in Full Stack</li><li>‘original’: Original Source</li><li>‘custom’: Original Source</li></ul></li><li>data.tileData</li><li>data.widgetId</li></ul> |
| beforeExpandedTileOpen \_(preventable)\_        | Triggered before the Expanded Tile opens                 | <ul><li>e</li><li>data.el</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                                                  |
| expandedTileOpen                                | Triggered after the Expanded Tile opens                  | <ul><li>e</li><li>data.el</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                                                  |
| expandedTileImageLoad                           | Triggered after the Expanded Tile image has been laoded  | <ul><li>e</li><li>data.el</li><li>data.tileEl</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                              |
| beforeExpandedTileImageResize \_(preventable)\_ | Triggered before the Expanded Tile image resizes         | <ul><li>e</li><li>data.sizes</li><li>data.el</li><li>data.tileEl</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                           |
| expandedTileImageResize                         | Triggered after the Expanded Tile image resizes          | <ul><li>e</li><li>data.sizes</li><li>data.el</li><li>data.tileEl</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                           |
| expandedTileClose                               | Triggered after the Expanded Tile closes                 | <ul><li>e</li><li>data.el</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                                                  |
| shareClick                                      | Triggered after the click of sharing buttons             | <ul><li>e</li><li>data.shareNetwork</li><li>data.tileData</li><li>date.widgetId</li></ul>                                                                                                                                                                                        |
| userClick                                       | Triggered after the click of the user links              | <ul><li>e</li><li>data.tileData</li><li>date.widgetId</li></ul>                                                                                                                                                                                                                  |
| moreLoad                                        | Triggered after the more tiles have been loaded          | <ul><li>e</li><li>data.page</li><li>date.widgetId</li></ul>                                                                                                                                                                                                                      |
| shopspotActionClick                             | Triggered after the Shopspot CTA button has been clicked | <ul><li>e</li><li>data.productTag</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                                          |
| shopspotFlyoutExpand                            | Triggered after the Shopspot Flyout opens                | <ul><li>e</li><li>data.productTag</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                                          |
| productActionClick                              | Triggered after the Product CTA button has been clicked  | <ul><li>e</li><li>data.productTag</li><li>data.tileData</li><li>data.widgetId</li></ul>                                                                                                                                                                                          |

### Methods

You can update Widget content by invoking the following methods.

#### Example Usage

```
Stackla.WidgetManager.search(widgetId, keyword)
```

Note: To avoid manually checking the existence of the `Stackla.WidgetManager` global variable\
we recommend you use the Expanded Tile Code Editor in our Nosto Admin Portal.

#### Available Methods

| Method Name            | Description                                                                                                                                                                                                                        | Arguments                                                                                                                      |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| changeFilter           | Trigger to update content by switching to another filter                                                                                                                                                                           | <ul><li>widgetId</li><li>filterId</li><li>tags (optional)</li><li>group (optional)</li></ul>                                   |
| destroy                | Trigger to \*\*destroy all\*\* the widgets on the current page. You can even remove the embed codes when you specify the first argument to \`true\`.                                                                               | <ul><li>isIncludeEmbedCode (optional)</li></ul>                                                                                |
| loadMore               | Trigger to load more data programmatically.                                                                                                                                                                                        | <ul><li>widgetId</li></ul>                                                                                                     |
| rebuild                | Trigger to \*\*rebuild all\*\* the widgets on the current page with the current \`data-\*\` attributes.                                                                                                                            |                                                                                                                                |
| search                 | <p>Trigger to update content by searching a keyword.</p><p>Note that you will need to make a request to<br>Nosto Support<br>to make this method available to work properly.<br>‘Text Search’ by default is not made available.</p> | <ul><li>widgetId</li><li>keyword</li></ul>                                                                                     |
| searchFilter           | This is an alias for \`search\`. \*\*It will be deprecated in the future.\*\*                                                                                                                                                      | <ul><li>widgetId</li><li>keyword</li></ul>                                                                                     |
| postMessage            | Post customized message to widget iframe(s).                                                                                                                                                                                       | <ul><li>type</li><li>data</li><li>widgetId (optional)</li></ul>                                                                |
| executeOnProductChange | When a product is changed on widgets with Shopspots, this method will be invoked.                                                                                                                                                  | <ul><li>method (method to be executed)</li></ul>                                                                               |
| injectHTML             | Inject HTML into all expanded tiles at the given location                                                                                                                                                                          | <ul><li>target_element (element to inject into)</li><li>content (injectable content)</li><li>location (before/after)</li></ul> |
| .\_waitForElement      | Wait for an element to render before performing functions. Requires promise usage.                                                                                                                                                 | <ul><li>element (element to wait on)</li></ul>                                                                                 |

#### More Details

Take a look at our[\
How to use “Filter and search” in a Widget](../../../guides/onsite-widgets/how-to-use-filter-and-search-in-widget.md) article for more examples.

Looking for some help with CSS widget customisations? Check out our CSS guides\
for styling the[\
Waterfall Widget](../../../guides/onsite-widgets/styling-waterfall-widget.md),[\
Carousel Widget](../../../guides/onsite-widgets/styling-carousel-widget.md), as well as the[\
Widget Expanded Tile](../../../guides/onsite-widgets/styling-widget-expanded-tile.md).
