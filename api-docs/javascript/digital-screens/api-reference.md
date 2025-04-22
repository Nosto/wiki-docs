---
description: Introduction to Customizing the Event Screens
---

# API Reference

* [API Specification](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#api-specs)
* [StacklaEventScreens.Base](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#base)
* [StacklaEventscreens.Carousel](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#ses-carousel)
* [StacklaEventscreens.Mosaic](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#ses-mosaic)
* [Additional Information](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#other)

### API Specification

This section is reserved for future specification of each area of the JavaScript API.

### StacklaEventscreens.Base

All the Event Screen type classes extend `StacklaEventScreens.Base`.

#### Configurable Options

| Name             | Default       | Description                                                                     |
| ---------------- | ------------- | ------------------------------------------------------------------------------- |
| appendRoot       | ‘#container’  | CSS selector of tiles being appended to.                                        |
| debug            | 0             | Whether or not to enable showing the debugging logs.                            |
| enableImageCheck | 0             | Whether or not to enable minimal image size filter.                             |
| listUrl          | <$list\_url>  | Data service URL.                                                               |
| filterId         | <$filter\_id> | Filter ID. You can switch to use a different filter to load different data set. |
| imageMinHeight   | null          | Minimum image height. It only takes effect when enableImageCheck is turned on.  |
| imageMinWidth    | null          | Minimum image width. It only takes effect when enableImageCheck is turned on.   |
| queueLength      | 30            | Available items in queue, not including pinned tiles.                           |
| template         | ‘#template’   | CSS selector of Tile Mustache template.                                         |

#### Properties

| Name         | Type           | Description                                            |
| ------------ | -------------- | ------------------------------------------------------ |
| dataProvider | \<StacklaData> | The utility facilitates data requests with API server. |

#### Methods

| Name    | Params               | Return    | Description                                                                                                                                                                                                                                                                                                                                            |
| ------- | -------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| log     | message, type        | (void)    | A convenient logging method. Only output to developer console when `debug` is enabled.                                                                                                                                                                                                                                                                 |
| emit    | eventName, args      | (void)    | Triggers an event.                                                                                                                                                                                                                                                                                                                                     |
| on      | eventName, handlerFn | (void)    | Subscribes to an event.                                                                                                                                                                                                                                                                                                                                |
| init    |                      | void      | Init lifecycle method, invoked during construction. Sets up attributes and invokes initializers for this Event Screen type.                                                                                                                                                                                                                            |
| bind    |                      | void      | Bind lifecycle method, invoked during construction. Binds event handlers for this Event Screen type.                                                                                                                                                                                                                                                   |
| render  | container, html      | (void)    | <p>Establishes the initial DOM for the Event Screen type.<br>Invoking this method will lead to the creating of all DOM elements for the Event Screen.<br>This method should only be invoked once for an initialized Event Screen.</p><ul><li>container (jQuery) – The Event Screen placeholder</li><li>html (String) – The HTML of all tiles</li></ul> |
| iterate | i, data              | \<String> | Invoked by render method. Focuses on generating HTML of an individual tile.                                                                                                                                                                                                                                                                            |

**Method Usage**

All Event Screen types have defined `init`, `bind`, `iterate`, and `render`\
lifecycle methods. You can overwrite it through prototype chain for any customization reasons.

For example, the Carousel Event Screen uses [slick.js](https://web.archive.org/web/20230130205754/http://kenwheeler.github.io/slick/) to achieve carousel effect.\
You may want to use another JavaScript utility to achieve this.

```
StacklaEventscreens.Carousel.prototype.render = function (container, html) {
    var slider = Sliderman.slider({
        container: container.attr('id'),
        // Other attributes....
    };
}
```

#### Events

| Name          | Params                       | Description                                                   |
| ------------- | ---------------------------- | ------------------------------------------------------------- |
| init          | instance                     | It triggers after an instance being initialized.              |
| load          | listData                     | It triggers after first data being received.                  |
| beforeRender  | $appendRoot, listData        | It triggers before starting to parse any tile HTML.           |
| beforeIterate | tileData, tileID             | It triggers before starting to parse an individual tile HTML. |
| iterate       | tileData, tileHTML           | It triggers after a tile HTML being generated.                |
| beforeAppend  | $tile, tileData, $appendRoot | It triggers before an individual tile being appended.         |
| append        | $tile, tileData, $appendRoot | It triggers after an individual tile being appended.          |
| render        | $appendRoot, listData        | It triggers after instance being initialized.                 |
| message       | addedData, removeIDList      | It triggers when new data being received.                     |

**Event Usage**

The recommended way of subscribing an event is to use the `on` method.

```
var eventScreen = new StacklaEventscreens.Wall();
eventScreen.on('append', function (e, $tile, $container) { /* do something… */  });
eventScreen.on('beforeAppend', function (e, $tile, $container) { /* do something… */  });
```

Also, all the above events can be subscribed via _configuration options_.

```
var eventScreen = new StacklaEventscreens.Wall({
    onInit: function (instance) {},
    onLoad: function (data) {},
    onIterate: function (tile, html) {},
    onRender: function (container, data, html) {},
    onMessage: function (data, ids) {}
};
```

[Back to Top](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#top)

### StacklaEventscreens.Carousel

#### Events

Except the events which Base provides, it also has the following unique events.

| Name             | Params                      | Description                      |
| ---------------- | --------------------------- | -------------------------------- |
| beforeTransition | slick, prevIndex, nextIndex | It triggers before a transition. |
| transition       | slick, index                | It triggers after a transition.  |

[Back to Top](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#top)

### StacklaEventscreens.Mosaic

#### Events

Except the events which Base provides, it also has the following unique events.

| Name             | Params                 | Description                      |
| ---------------- | ---------------------- | -------------------------------- |
| beforeTransition | $prevGroup, $nextGroup | It triggers before a transition. |
| transition       | $nextGroup             | It triggers after a transition.  |

[Back to Top](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#top)

### Additional Information

#### Sample Code

Say if you want to make a custom eventscreen called Zoom Wall, you can extend StacklaEventscreens.Base.

_stackla\_eventscreens\_zoomwall.js_

```
function ZoomWall(attrs) {
      StacklaEventscreens.Base.call(this, attrs);
      // Define some attributes you want to use
      this.delay = attrs.delay || 5000;
    }
    ZoomWall.prototype = Object.create(StacklaEventscreens.Base.prototype);
    ZoomWall.prototype.constructor = ZoomWall;
    ZoomWall.prototype.NAME = 'stackla.eventscreens.zoomwall';
    ZoomWall.prototype.init = function () {
      // Maybe handles the new arriving messages.
      this.on('message', function (e) {
        // Update tiles...
      });
    };
    ZoomWall.prototype.render = function (container, html) {
      // Maybe update HTML structure in container.
      container.html('<div class="content">' + html + '</div>');
    };
    StacklaEventscreens.ZoomWall = ZoomWall;
```

[Back to Top](https://web.archive.org/web/20230130205754/https://developer.stackla.com/api-docs/javascript/events/reference/#top)
