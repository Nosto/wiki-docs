---
description: JavaScript API for Blank Canvas Widget
---

# API Reference for Blank Canvas

{% hint style="warning" %}
You are reading the Classic Widget Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../../../guides/widgets-nextgen/typescript-api-guides/how-to-use-the-sdk.md).


{% endhint %}

* [Setting Object](blank-canvas.md#setting-object)
* [Methods](blank-canvas.md#methods)
  * [Sample Usage](blank-canvas.md#helper-methods-sample-usage)
  * [Tile Properties](blank-canvas.md#helper-methods-tile-properties)

## Setting Object (Read-only)

In our Blank Canvas Widget, you can access the widget setting configuration by checking the `window.Stackla.widgetOptions` global variable. The following table lists useful properties.

| Property Name         | Type     | Description                                                       |
| --------------------- | -------- | ----------------------------------------------------------------- |
| custom\_css           | {String} | Compiled Custom CSS for Inline Tile                               |
| custom\_js            | {String} | Source Custom JavaScript for Inline Tile                          |
| custom\_tile          | {String} | Custom Template in Mustache syntax for Inline Tile                |
| filter\_id            | {String} | ID of selected filter                                             |
| guid                  | {String} | A hash code representing unique Widget ID (Widget ID replacement) |
| id                    | {String} | Widget ID                                                         |
| lightbox\_custom\_css | {String} | Compiled Custom CSS for Expanded Tile                             |
| lightbox\_source\_css | {String} | Source Custom CSS in LESS syntax for Expanded Tile                |
| slug                  | {String} | Widget Domain                                                     |
| source\_css           | {String} | Source Custom CSS in LESS syntax for Inline Tile                  |
| stack\_id             | {Number} | Stack ID                                                          |
| short\_name           | {String} | Short name of current Stack                                       |
| style.name            | {String} | Name of your Widget                                               |

## Methods

To ease the pain of building a widget from scratch, we've provided some methods for you.

| Method Name               | Arguments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Return Value                                                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Stackla.loadTilesByFilter | <p></p><ol><li><p>options {Object} (optional)</p><ul><li>list_url: '/api/widget'</li><li>pin_url: '/api/pins'</li><li>page: 1</li><li>limit: 30</li><li>filter_id: widgetOptions.filter_id</li><li>polling: 10000</li><li>ttl: 30</li><li>tags: '56983,56782'</li><li>tags_grouped_as: 'AND'</li><li>tile_id: '5828c80b2f1f77812ecbbe68'</li><li>search: 'justinbieber'</li></ul></li></ol><ol start="2"><li><p>callback {Funciton} (optional)</p><ul><li>data</li></ul></li></ol>                                              | <p></p><p>$.Deferred</p><ul><li>data</li></ul>                                    | <p></p><p>Loads tiles data from the selected filter in Widget edit page (Settings > Select Filter)</p><ul><li>list_url: Tiles API URL.</li><li>pin_url: Pinned tiles API URL.</li><li>page: Page number for Tiles API URL.</li><li>limit: Page size for Tiles API URL.</li><li>filter_id: Nosto's UGC Filter ID.</li><li>polling: Polling interval in millionseconds.</li><li>ttl: Tile to live in seconds. It's for cache purpose.</li><li>tags: Comma-separated Tag IDs.</li><li>tags_grouped_as: Tags grouping rule. The value can be 'and' or 'or'.</li><li>tile_id: Comma-separad Tile IDs.</li><li>search: Search keyword.</li></ul>                                                                                                                                                                                                                       |
| Stackla.render            | <p></p><ol><li>data {Object} (<strong>required</strong>)</li></ol><ol start="2"><li><p>options {Object} (<strong>optional</strong>)</p><ul><li>$container: $(document.body)</li><li>auto_resize: true</li><li>template: $('#tpl-layout').html()</li><li>setting: Stackla.widgetOptions</li><li><p>renderer</p><ul><li>data</li><li>options</li><li>templates</li></ul></li></ul></li></ol><ol start="3"><li><p>callback {Funciton} (<strong>optional</strong>)</p><ul><li>html</li><li>data</li><li>options</li></ul></li></ol> | <p></p><p> $.Deferred</p><ul><li>data</li><li>options</li><li>templates</li></ul> | <p></p><p>Renders the layout template by provided data and outputs it on page.</p><p>The following lists the details for the optional <code>options</code> object.</p><ul><li>$container: The container which the html being prepended to. The default value is <code>$('document.body')</code>.</li><li>auto_resize: Indicates if the it adjusts the iframe dimension automatically. The default value is <code>true</code>.</li><li>template: The layout template in Mustache. The default value is <code>$('#tpl-layout').html()</code></li><li>setting: The widget configuration. The default value is <code>Stackla.widgetOptions</code>.</li><li><p>renderer: You can customize the rendering.</p><ul><li>data: The tiles data.</li><li>options: This <code>options</code> object.</li><li>templates: The Mustache templates in array.</li></ul></li></ul> |
| Stackla.expandTile        | <p></p><ol><li>tileData {Object} (<strong>required</strong>)</li><li>expandType {String} (<strong>optional</strong>)</li></ol>                                                                                                                                                                                                                                                                                                                                                                                                  | void                                                                              | Displays the Expanded Tile by providing the tile data. You can specify `portrait` or `landscape` as expand type. Also other options to control what to show, `show_products` `show_shopspots` and `show_caption`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

### Sample Usage

#### Using Callback

You can use old school callback.

```
Stackla.loadTilesByFilter(function (tiles, dataFilter) {
    Stackla.render({tiles: tiles});
});
```

#### Using jQuery Deferred

Or use the modern Promise-style way.

```
Stackla.loadTilesByFilter()
.then(function (tiles, dataFilter) {
    Stackla.render({tiles: tiles});
});
```

#### Adjusting Options

You can adjust the request parameters by providing options object.

```
 // Using jQuery Deferred
Stackla.loadTilesByFilter({filter_id: 1234, page: 2})
.then(function (tiles) {
    Stackla.render({tiles: tiles});
});

// Or using tranditional callback
Stackla.loadTilesByFilter({filter_id: 1234, page: 2}, function (tiles) {
    Stackla.render({tiles: tiles});
});
```

#### Customized Rendering

You can render the widget according to your needs.

```
Stackla.loadTilesByFilter().then(function (tiles) {
    Stackla.render({tiles: tiles}, {
        auto_resize: false, // Handle the iframe resizing by yourself
        renderer: function (data, options, templates) {
            // Render the HTML
            var html = Mustache.render(options.template, data, templates);
            console.log(options.template, data, templates);

            // Create a wrapper instead of appending to body directly.
            $wrapper = $('<div class="wrapper"/>');
            $wrapper.html(html);
            $(document.body).prepend($wrapper);

            // Adjust the viewport height manually since I set auto_resize to false.
            Stackla.postMessage('resize', {height: $('body').height()});

            return html;
        }
    });
});
```

#### Fetching Extra Data

You can load your own data.

```
$.when(
    Stackla.loadTilesByFilter(),
    yourExtraDataRequest()
).then(function (tilesResponse, extraDataResponse) {
    Stackla.render({
        tiles: tilesResponse[0],
        extra: extraDataResponse[0]
    });
});
```

#### Config Expanded Tile

You can customize what to show on expanded tile.

<pre><code>Stackla.expandTile(data.tiles[$(this).attr("tileInnerID")],{
<strong>        layout: 'landscape',
</strong>        show_shopspots: true,
        show_products: true,
        show_caption: true
}
</code></pre>

### Tile Properties

You can use all of the following properties in both your JavaScript and Tile Template.

| Property Name             | Type       | Description                                                                                                       |
| ------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------- |
| anonymous                 | {Boolean}  | `true` if this tile doesn't display both user avatar and name. Only Custom Tile would set this property to `true` |
| avatar                    | {String}   | Avatar URL                                                                                                        |
| avatars                   | {Object}   | Different avatar sizes including L, M, S, XL, and XS                                                              |
| caption                   | {String}   | Parsed message                                                                                                    |
| claimed                   | {Boolean}  | `true` if this tile is being claimed                                                                              |
| class\_names              | {String}   | Default CSS class names which should be added to tile wrapper element                                             |
| comments                  | {Array}    | Comment list                                                                                                      |
| competitions              | {Array}    | Competition list                                                                                                  |
| content\_only             | {Boolean}  | `true` if this tile doesn't display anything except content. Only Custom Tile would set this property to `true`   |
| created\_at               | {Number}   | The timestamp of Nosto's UGC ingestion                                                                            |
| downs                     | {Array}    | Dislike list                                                                                                      |
| emoji                     | {Function} | The lambda for converting Emoji unicode to images, which is useful in Mustache.                                   |
| has\_avatar               | {Boolean}  | Indicates if this tile has an user avatar                                                                         |
| has\_image                | {Boolean}  | Indicates if this tile has an image                                                                               |
| has\_image\_dimension     | {Boolean}  | Indicates if the tile image has dimension saved in Nosto's UGC                                                    |
| has\_title                | {Boolean}  | Indicates if the tile has a title                                                                                 |
| has\_video                | {Boolean}  | Indicates if the tile has a video                                                                                 |
| height\_ratio             | {Number}   | The ratio of image height                                                                                         |
| html                      | {String}   | Tile message in HTML (not always available)                                                                       |
| id                        | {String}   | Tile ID                                                                                                           |
| image                     | {String}   | Image URL                                                                                                         |
| image\_thumbnail\_url     | {String}   | A smaller (400px) and optimised version of image URL                                                              |
| image\_alt\_text          | {String}   | Image alternative text                                                                                            |
| image\_max                | {String}   | Large image URL                                                                                                   |
| image\_max\_height        | {Number}   | Large image height                                                                                                |
| image\_max\_width         | {Number}   | Large image width                                                                                                 |
| is\_ecal                  | {Boolean}  | Indicates if it's an ECal tile                                                                                    |
| is\_firefox               | {Boolean}  | Indicates if user's browser is Firefox                                                                            |
| is\_gif\_video            | {Boolean}  | Indicates if it's a GIF video tile                                                                                |
| is\_ie8                   | {Boolean}  | Indicate if user's browser is IE8                                                                                 |
| is\_pin                   | {Boolean}  | indicate if it's an pinned tile                                                                                   |
| is\_stackla\_feed         | {Boolean}  | Indicate if it's a stackla feed                                                                                   |
| is\_video                 | {Boolean}  | Indicate if it's a video                                                                                          |
| is\_winner                | {Boolean}  | Indicate if it's a winner tile for competition                                                                    |
| lang                      | {String}   | Detected language code                                                                                            |
| media                     | {String}   | Media type. The value could be `image`, `video`, or `text`                                                        |
| message                   | {String}   | Tile message                                                                                                      |
| numComments               | {Number}   | The amount of comments                                                                                            |
| numDowns                  | {Number}   | The amount of down (dislike/vote)                                                                                 |
| numQueuedComments         | {Number}   | The amount of queued comments                                                                                     |
| numUps                    | {Number}   | The amount of up (like/vote)                                                                                      |
| numVotes                  | {Number}   | The amount of vote                                                                                                |
| original\_link            | {String}   | The URL of original post                                                                                          |
| original\_url             | {String}   | The URL of original post                                                                                          |
| replies                   | {Array}    | Comment replies list                                                                                              |
| score                     | {Number}   | Like/Dislike Score                                                                                                |
| source                    | {String}   | Source media name such as `rss`, `sta_feed`, `twtiter` and so on                                                  |
| source\_created\_at       | {Number}   | Creation timestamp of original post                                                                               |
| source\_user\_id          | {String}   | Creation User ID in source website                                                                                |
| stackla\_sentiment\_score | {Number}   | Sentimental score                                                                                                 |
| tag\_class\_names         | {String}   | CSS class names which should be added to tile wrapper element                                                     |
| tag\_id\_list             | {String}   | Tag ID list which is separated by comma                                                                           |
| tag\_name\_list           | {String}   | Tag name list which is separated by comma                                                                         |
| tags                      | {Array}    | Tag ID array                                                                                                      |
| tags\_extended            | {Array}    | Extended tag data                                                                                                 |
| term\_meta                | {Array}    | Aggregation terms meta                                                                                            |
| terms                     | {String}   | Aggregation terms list which is separated by comma                                                                |
| time\_phrase              | {String}   | Time format 1. ex. "10 Dec"                                                                                       |
| timephrase                | {String}   | Time format 2. ex. "2 weeks ago"                                                                                  |
| title                     | {String}   | Tile title                                                                                                        |
| updated\_at               | {Number}   | Last updated timestamp                                                                                            |
| ups                       | {Array}    | Up list of like/dislike                                                                                           |
| user                      | {String}   | User handle                                                                                                       |
| user\_link                | {String}   | User URL of source website                                                                                        |
| vd                        | {Boolean}  | Visible on Desktop                                                                                                |
| ve                        | {Boolean}  | Visible on Eventscreen                                                                                            |
| via\_source               | {String}   | Same with `source`                                                                                                |
| mp4\_url                  | {String}   | Video URL                                                                                                         |
| vm                        | {Boolean}  | Visible on mobile                                                                                                 |
| vote\_id                  | {String}   | Vote ID                                                                                                           |
| voted\_competition        | {Boolean}  | Indicates if current user has voted for competition.                                                              |
| votes                     | {Array}    | Vote list                                                                                                         |
| vw                        | {Boolean}  | Visible on Widget                                                                                                 |
| width\_ratio              | {Number}   | Width ratio                                                                                                       |
