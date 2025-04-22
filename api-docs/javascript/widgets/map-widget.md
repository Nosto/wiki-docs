---
description: JavaScript API for Map Widget
---

# API Reference for Map Widget

* [Background](map-widget.md#background)
  * [Geohash](map-widget.md#background-geohash)
  * [Execution flows](map-widget.md#background-execution-flows)
* [Methods](map-widget.md#methods)
  * [.initialize(options)](map-widget.md#method-initialize)
  * [.createMap()](map-widget.md#method-createMap)
  * [.createLayer(style)](map-widget.md#method-createLayer)
  * [.handleMarkerClick(event)](map-widget.md#method-handleMarkerClick)
  * [.handleMoveEnd()](map-widget.md#method-handleMoveEnd)
  * [.createMarkerBySummary(summary)](map-widget.md#method-createMarkerBySummary)
  * [.on(eventName, handler)](map-widget.md#method-on)
  * [.getAggregatedDataByBounds(sw, ne, numGeohashPerView)](map-widget.md#method-getAggregatedDataByBounds)
* [Exported methods](map-widget.md#exported-methods)
  * [.getAggregatedDataByBounds(sw, ne, numGeohashPerView)](map-widget.md#exported-method-getAggregatedDataByBounds)
* [Customization examples](map-widget.md#customization-examples)
  * [Add logic before/after the map widget starts](map-widget.md#customization-example-add-logic-before-after-map-widget-starts)
  * [Initialize Leaflet with custom options](map-widget.md#customization-example-initialize-leaflet-with-custom-options)
  * [Use a different map layer (e.g. Google Maps)](map-widget.md#customization-example-use-a-different-map-layer-eg-google-maps)

## Background

The Map plugin’s purpose is to provide a visualization of where content is happening.\
This adds value when the location of content is as important as the content itself.

* The Mapping Widget overlays geo-filtered content as Pins on a map;
* The user sets the initial viewing area
* The user can choose from various mapping providers – Google Maps, Mapzen, CartoDB and OpenStreetMap
* They can customize the appearance of the Map based on which provider they choose
* Each Pin reflects a Tile or a cluster (group) of Tiles
* The Pin size is relative to the cluster size. The bigger the Pin, the more content that it represents.
* The user can zoom in and out on the map to see a more refined location of the Pins

### Geohash

Tiles in the Maps Widget are grouped into clusters using Geohash. Geohash is a string representation of coordinates.

> Geohash is a geocoding system invented by Gustavo Niemeyer and placed into the public domain. It is a\
> hierarchical spatial data structure which subdivides space into buckets of grid shape, which is one of the many\
> applications of what is known as a Z-order curve, and generally space-filling curves.

Reference [Wikipedia: Geohash](https://en.wikipedia.org/wiki/Geohash)

### Execution flows

#### Map widget creation

1. MapWidget is created. `var widget = new MapWidget()`
2. Custom JS code is executed `// custom javascript`
3. The widget is initialized. `widget.initialize()`
   1. Leaflet map is created. `var map = widget.createMap()`
   2. Map layer is created. `map.setLayer(widget.createLayer())`

#### On map move

1. On move, the end is called. `widget.handleMoveEnd()`
   1. Get all tile summaries for the current visible area. `var summaries = widget.getAggregatedDataByBounds()`
   2. Render the fetched summaries. `widget.renderSummaries(summaries)`

#### On marker clicked

1. On marker, click is called. `widget.handleMarkerClick()`

## Methods

### .initialize(options)

Initialize the map widget.

**Parameters**

| Name    | Type       | Description |
| ------- | ---------- | ----------- |
| options | \`Object\` | Options     |

**Return**\
`Promise` A promise that resolves when initialization completes.

### .createMap()

Creates a Leaflet Map object.

**Return**\
`L.Map` The created map object.

### .createLayer(style)

Creates a Leaflet Map object.

**Parameters**

| Name  | Type       | Description  |
| ----- | ---------- | ------------ |
| style | \`Object\` | Style object |

**Return**\
`L.Layer` The created map object.

### .handleMarkerClick(event

Handles marker click. The default implementation is updates the content list if it is connected to a\
content widget. Otherwise, it only emits the ‘marker:click’ event which can be listen using [`.on`](map-widget.md#method-on)

**Parameters**

| Name  | Type      | Description |
| ----- | --------- | ----------- |
| event | \`Event\` | Click event |

**Return**\
`void`

#### Example usage

```
widget.handleMarkerClick = function (event) {
    var $dom = $(event.originalEvent.target);
    // do something

    // call parent method (optional)
    Widget.prototype.handleMarkerClick.call(widget, event);
};
```

### .handleMoveEnd()

Handles map panning, resizing and zooming.

**Return**\
`void`

### .createMarkerBySummary(summary)

Creates a Leaflet Map object.

**Parameters**

| Name    | Type                       | Description |
| ------- | -------------------------- | ----------- |
| summary | <pre><code>{
</code></pre> |             |

```
geohash:String,––
quantity:Integer,
avg_location:{
    longitude:Float,
    latitude:Float
}
```

} | Summary object |

**Return** `L.Marker` The marker object.

### .on(eventName, handler)

Listens to an event.

**Parameters**

| Name      | Type                           | Description        |
| --------- | ------------------------------ | ------------------ |
| eventName | \`String\`                     | Event name         |
| handler   | \`Function(data:Object):void\` | A handler function |

**Return**\
`MapWidget` The current map widget object.

#### Example usage

```
// Event 'marker:click' might not emit if handleMoveEnd() or handleMarkerClick() is overrode.
widget.on('marker:click', function (summary) {
    console.log('Geohash: ' + summary.geohash);
    console.log('Quantity: ' + summary.quantity);
    console.log('Average longitude: ' + summary.avg_location.longitude);
    console.log('Average latitude: ' + summary.avg_location.latitude);
});
```

### .getAggregatedDataByBounds(sw, ne, numGeohashPerView)

Listens to an event.

**Parameters**

| Name              | Type                       | Description                                                                       |
| ----------------- | -------------------------- | --------------------------------------------------------------------------------- |
| sw                | \`{lat:Float, lng:Float}\` | South west coordinates                                                            |
| ne                | \`{lat:Float, lng:Float}\` | North east coordinates                                                            |
| numGeohashPerView | \`Integer\`                | (Optional) Max number of geohashes to generate in the given view. Default: \`32\` |

**Return**\
`Promise<Summary[]>` A promise that resolves to summaries.

## Exported methods

Exported methods can be called in parent window.

### .getAggregatedDataByBounds(sw, ne, numGeohashPerView)

See [.getAggregatedDataByBounds(sw, ne, numGeohashPerView)](map-widget.md#method-getAggregatedDataByBounds)

#### Example usage (own Google Maps)

```javascript
    var map; // Google Map instance
    // get the map widget by ID
    var mapWidget = Stackla.WidgetManager.getWidgetInstanceByWidgetId(1234);
    map.addListener('bounds_changed', function () {
        var bounds = map.getBounds();
        var sw = bounds.getSouthWest();
        var ne = bounds.getNorthEast();
        mapWidget.getAggregatedDataByBounds(
            {lat: sw.lat(), lng: sw.lng()},
            {lat: ne.lat(), lng: ne.lng()},
            16
        ).then(function (summaries) {
    
        }).catch(function (ex) {
            console.error(ex);
        });
    });
```

## Customization examples

### Add logic before/after map widget starts.

It is possible to add logic before and after the widget initializes. Below is an example using [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise).

```javascript
widget.initialize = function (options) {
        (new Promise(function (resolve, reject) {
            try {
                // perform necessary options before widget initialization starts.
                // ...
                // ...

                // at the end, call resolve()
                resolve();
            } catch (ex) {
                reject(ex);
            }
        }).then(function () {
            return MapWidget.prototype.initialize.call(widget, options);
        }).then(function () {
            // then perform actions after widget initialization is completed.
        }).catch(function (ex) {
            // catch error/exception
            console.error(ex);
        });
    };
```

### Initialize Leaflet with custom options

```javascript
// Leaflet options @see http://leafletjs.com/reference-1.0.0.html#map-factory
    widget.createMap = function () {
        var map = L.map('map', {
            maxBounds: [
                [-85.0, -180.0],
                [85.0, 180.0]
            ],
            scrollWheelZoom: false
        });
        return map;
    };
```

### Use a different map layer (e.g. Google Maps)

```javascript
// You can create your own Google Map style here https://mapstyle.withgoogle.com/
    widget.createLayer = function (style) {
        return L.gridLayer.googleMutant({
            styles: styles,
            type: 'roadmap'
        });
    };
```
