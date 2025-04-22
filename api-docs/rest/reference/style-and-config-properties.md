# REST API Reference Widgets style and config

* [Gallery (base\_gallery)](style-and-config-properties.md#widgets-base_gallery)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_gallery)
  * [`config` properties](style-and-config-properties.md#widgets-config-base_gallery)
* [Waterfall (base\_waterfall)](style-and-config-properties.md#widgets-base_waterfall)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_waterfall)
  * [`config` properties](style-and-config-properties.md#widgets-config-base_waterfall)
* [Carousel (base\_carousel)](style-and-config-properties.md#widgets-base_carousel)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_carousel)
  * [`config` properties](style-and-config-properties.md#widgets-config-base_carousel)
* [Slideshow (base\_slideshow)](style-and-config-properties.md#widgets-base_slideshow)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_slideshow)
* [Billboard (base\_billboard)](style-and-config-properties.md#widgets-base_billboard)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_billboard)
* [Feed (base\_feed)](style-and-config-properties.md#widgets-base_feed)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_feed)
* [Blank Canvas (base\_blankcanvas)](style-and-config-properties.md#widgets-base_blankcanvas)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_blankcanvas)
* [Map Leaflet (base\_map\_leaflet)](style-and-config-properties.md#widgets-base_map_leaflet)
  * [`style` properties](style-and-config-properties.md#widgets-style-base_map_leaflet)

## Widget Style and Config

This area provides detail around each of the available Styles for widget.

## Gallery (base\_gallery)

### `style` Properties

| Field                      | Type     | Value       | POST | PUT | Definition                                                            |
| -------------------------- | -------- | ----------- | ---- | --- | --------------------------------------------------------------------- |
| click\_through\_url        | string   | \[EXPAND]   |      |     | Click through URL                                                     |
| layout\_mode               | string   | `square`    |      |     | Layout mode. The value will need to be one of the following: `square` |
| margin                     | interger | 10          |      |     | Size of margin between tile                                           |
| name                       | string   | Gallery     |      |     | Name of widget                                                        |
| rows                       | interger | 3           |      |     | Size of margin between tile                                           |
| show\_powered\_by\_stackla | boolean  | true        |      |     | Whether to show the Powered by Nosto's UGC icon                       |
| tile\_size                 | string   | medium      |      |     | Tile size. The value will need to be one of the following: `small`    |
| widget\_background         | string   | transparent |      |     | Hex code of color                                                     |

[Back to Top](style-and-config-properties.md#top)

### `config` Properties

#### `config.lightbox` properties

| Field                                          | Type    | Value    | POST | PUT | Definition                                                                        |
| ---------------------------------------------- | ------- | -------- | ---- | --- | --------------------------------------------------------------------------------- |
| apply\_custom\_sharing\_title\_on\_miss\_title | boolean | false    |      |     | Custom title for sharing                                                          |
| disable\_short\_url                            | boolean | false    |      |     |                                                                                   |
| layout                                         | string  | portrait |      |     | Lightbox layout style. The value will need to be one of the following: `portrait` |
| sharing\_text                                  | string  |          |      |     |                                                                                   |
| sharing\_title                                 | string  |          |      |     |                                                                                   |
| show\_additional\_info                         | boolean | true     |      |     | Toggle to show and hide additional info                                           |
| show\_comments                                 | boolean | false    |      |     | Toggle to show and hide comments                                                  |
| show\_sharing                                  | boolean | false    |      |     | Toggle to show and hide sharing                                                   |
| show\_shopspots                                | boolean | false    |      |     | Toggle to show and hide sharing in inline tile                                    |
| show\_tags                                     | boolean | false    |      |     | Toggle to show and hide tags                                                      |

## Waterfall (base\_waterfall)

### `style` Properties

Hex code of color

| Field                                 | Type     | Value                                          | POST | PUT | Definition                                                               |
| ------------------------------------- | -------- | ---------------------------------------------- | ---- | --- | ------------------------------------------------------------------------ |
| auto\_refresh                         | boolean  | false                                          |      |     |                                                                          |
| click\_through\_url                   | string   | \[EXPAND]                                      |      |     | Click through URL                                                        |
| domain                                | string   |                                                |      |     |                                                                          |
| enable\_custom\_tiles\_per\_page      | integer  | false                                          |      |     | Number of tile per page load                                             |
| enable\_doublecolumnspan              | boolean  | false                                          |      |     |                                                                          |
| load\_more\_type                      | string   | `button`                                       |      |     | Load more type. The value will need to be one of the following: `scroll` |
| enable\_typekit                       | boolean  | false                                          |      |     | Flag to enable typekit                                                   |
| margin                                | interger | 0                                              |      |     | Size of margin between tile                                              |
| max\_tile\_width                      | interger | 365                                            |      |     | Maximum tile width                                                       |
| name                                  | string   | Waterfall                                      |      |     | Name of widget                                                           |
| parent\_page\_ext\_username           | boolean  | false                                          |      |     |                                                                          |
| parent\_page\_permission              | boolean  | false                                          |      |     |                                                                          |
| parent\_page\_secret\_key             | string   |                                                |      |     |                                                                          |
| polling\_frequency                    | integer  | 30                                             |      |     | The frequency of polling contents in seconds                             |
| row\_per\_page                        | integer  | 3                                              |      |     | Number of rows per page load                                             |
| style                                 | string   | base\_waterfall                                |      |     | Widget style type.                                                       |
| text\_tile\_background                | string   | ffffff                                         |      |     | Hex code of color                                                        |
| text\_tile\_font\_color               | string   | 666666                                         |      |     | Hex code of color                                                        |
| text\_tile\_font\_size                | integer  | 24                                             |      |     | Font size for text                                                       |
| text\_tile\_user\_handle\_font\_color | string   | 333333                                         |      |     | Hex code of color                                                        |
| text\_tile\_user\_handle\_font\_size  | integer  | 18                                             |      |     | Font size for text                                                       |
| text\_tile\_user\_name\_font\_color   | string   | 333333                                         |      |     | Hex code of color                                                        |
| text\_tile\_user\_name\_font\_size    | integer  | 18                                             |      |     | Font size for text                                                       |
| tiles\_per\_page                      | integer  | 15                                             |      |     | Number of tiles per load                                                 |
| type                                  | string   | fluid                                          |      |     | Widget type                                                              |
| widget\_background                    | string   |                                                |      |     |                                                                          |
| widget\_loading\_image                | string   | http://sdp-media.dev/images/widget/loading.gif |      |     | URL of the widget loading image                                          |

[Back to Top](style-and-config-properties.md#top)

### `config` Properties

| Field         | Type                                                                                | Value | POST | PUT | Definition |
| ------------- | ----------------------------------------------------------------------------------- | ----- | ---- | --- | ---------- |
| tile\_options | [object](style-and-config-properties.md#widgets-config.tile_options-base_waterfall) |       |      |     |            |
| lightbox      | [object](style-and-config-properties.md#widgets-config.lightbox-base_waterfall)     |       |      |     |            |
| claim\_config | [object](style-and-config-properties.md#widgets-config.claim_config-base_waterfall) |       |      |     |            |

#### `config.tile_options` properties

| Field           | Type    | Value | POST | PUT | Definition                                     |
| --------------- | ------- | ----- | ---- | --- | ---------------------------------------------- |
| show\_comments  | boolean | false |      |     | Toggle to show and hide tags in inline tile    |
| show\_dislikes  | boolean | false |      |     | Toggle to show and hide dislike in inline tile |
| show\_likes     | boolean | false |      |     | Toggle to show and hide like in inline tile    |
| show\_sharing   | boolean | false |      |     | Toggle to show and hide sharing in inline tile |
| show\_shopspots | boolean | false |      |     | Toggle to show and hide sharing in inline tile |
| show\_tags      | boolean | false |      |     | Toggle to show and hide tags in inline tile    |
| show\_votes     | boolean | false |      |     | Toggle to show and hide votes in inline tile   |

#### `config.lightbox` properties

| Field                                          | Type    | Value    | POST | PUT | Definition                                                                        |
| ---------------------------------------------- | ------- | -------- | ---- | --- | --------------------------------------------------------------------------------- |
| apply\_custom\_sharing\_title\_on\_miss\_title | boolean | false    |      |     | Custom title for sharing                                                          |
| disable\_short\_url                            | boolean | false    |      |     |                                                                                   |
| layout                                         | string  | portrait |      |     | Lightbox layout style. The value will need to be one of the following: `portrait` |
| post\_comments                                 | boolean | false    |      |     | Toggle to allow user to add comment in lightbox                                   |
| sharing\_text                                  | string  |          |      |     |                                                                                   |
| sharing\_title                                 | string  |          |      |     |                                                                                   |
| show\_additional\_info                         | boolean | true     |      |     | Toggle to show and hide additional info                                           |
| show\_comments                                 | boolean | false    |      |     | Toggle to show and hide comments                                                  |
| show\_dislikes                                 | boolean | false    |      |     | Toggle to show and hide dislike                                                   |
| show\_likes                                    | boolean | false    |      |     | Toggle to show and hide likes                                                     |
| show\_nav                                      | boolean | false    |      |     | Toggle to show and hide navigation                                                |
| show\_sharing                                  | boolean | false    |      |     | Toggle to show and hide sharing                                                   |
| show\_shopspots                                | boolean | false    |      |     | Toggle to show and hide sharing in inline tile                                    |
| show\_tags                                     | boolean | false    |      |     | Toggle to show and hide tags                                                      |
| show\_votes                                    | boolean | false    |      |     | Toggle to show and hide votes                                                     |

#### `config.claim_config` properties

| Field                 | Type    | Value | POST | PUT | Definition                             |
| --------------------- | ------- | ----- | ---- | --- | -------------------------------------- |
| show\_claim\_button   | boolean | false |      |     | toggle to show and hide claim button   |
| show\_content\_toggle | boolean | false |      |     | toggle to show and hide content toggle |

[Back to Top](style-and-config-properties.md#top)

## Carousel (base\_carousel)

### `style` Properties

Number of rows per page load    Hex code of color     Hex code of color

| Field                            | Type    | Value                                          | POST | PUT | Definition                                                               |
| -------------------------------- | ------- | ---------------------------------------------- | ---- | --- | ------------------------------------------------------------------------ |
| click\_through\_url              | string  | \[EXPAND]                                      |      |     | Click through URL                                                        |
| enable\_custom\_tiles\_per\_page | string  |                                                |      |     |                                                                          |
| load\_more\_type                 | string  | `static`                                       |      |     | Load more type. The value will need to be one of the following: `button` |
| margin                           | integer | 7                                              |      |     | Tile margin                                                              |
| name                             | string  | Carousel                                       |      |     | Widget name                                                              |
| parent\_page\_secret\_key        | string  |                                                |      |     |                                                                          |
| polling\_frequency               | integer | 30                                             |      |     | The frequency of polling contents in seconds                             |
| row\_per\_page                   | string  |                                                |      |     |                                                                          |
| style                            | string  | base\_carousel                                 |      |     | Widget style type.                                                       |
| text\_tile\_background           | string  |                                                |      |     |                                                                          |
| text\_tile\_font\_color          | string  | 666666                                         |      |     | Hex code of color                                                        |
| text\_tile\_source\_color        | string  | 333333                                         |      |     | Hex code of color                                                        |
| tiles\_per\_page                 | integer | 15                                             |      |     | Number of tile per page load                                             |
| type                             | string  | fluid                                          |      |     |                                                                          |
| widget\_background               | string  |                                                |      |     |                                                                          |
| widget\_height                   | integer | 300                                            |      |     | Widget height                                                            |
| widget\_loading\_image           | string  | http://sdp-media.dev/images/widget/loading.gif |      |     | URL of the widget loading image                                          |

[Back to Top](style-and-config-properties.md#top)

### `config` Properties

| Field         | Type                                                                               | Value | POST | PUT | Definition |
| ------------- | ---------------------------------------------------------------------------------- | ----- | ---- | --- | ---------- |
| tile\_options | [object](style-and-config-properties.md#widgets-config.tile_options-base_carousel) |       |      |     |            |
| lightbox      | [object](style-and-config-properties.md#widgets-config.lightbox-base_carousel)     |       |      |     |            |
| claim\_config | [object](style-and-config-properties.md#widgets-config.claim_config-base_carousel) |       |      |     |            |

#### `config.tile_options` properties

| Field           | Type    | Value | POST | PUT | Definition                                     |
| --------------- | ------- | ----- | ---- | --- | ---------------------------------------------- |
| show\_comments  | boolean | false |      |     | Toggle to show and hide tags in inline tile    |
| show\_dislikes  | boolean | false |      |     | Toggle to show and hide dislike in inline tile |
| show\_likes     | boolean | false |      |     | Toggle to show and hide like in inline tile    |
| show\_sharing   | boolean | false |      |     | Toggle to show and hide sharing in inline tile |
| show\_shopspots | boolean | false |      |     | Toggle to show and hide sharing in inline tile |
| show\_tags      | boolean | false |      |     | Toggle to show and hide tags in inline tile    |
| show\_votes     | boolean | false |      |     | Toggle to show and hide votes in inline tile   |

#### `config.lightbox` properties

| Field                  | Type    | Value    | POST | PUT | Definition                                                                        |
| ---------------------- | ------- | -------- | ---- | --- | --------------------------------------------------------------------------------- |
| disable\_short\_url    | boolean | false    |      |     |                                                                                   |
| layout                 | string  | portrait |      |     | Lightbox layout style. The value will need to be one of the following: `portrait` |
| post\_comments         | boolean | false    |      |     | Toggle to allow user to add comment in lightbox                                   |
| sharing\_text          | string  |          |      |     |                                                                                   |
| sharing\_title         | string  |          |      |     |                                                                                   |
| show\_additional\_info | boolean | true     |      |     | Toggle to show and hide additional info                                           |
| show\_comments         | boolean | false    |      |     | Toggle to show and hide comments                                                  |
| show\_dislikes         | boolean | false    |      |     | Toggle to show and hide dislike                                                   |
| show\_likes            | boolean | false    |      |     | Toggle to show and hide likes                                                     |
| show\_nav              | boolean | false    |      |     | Toggle to show and hide navigation                                                |
| show\_product          | boolean | false    |      |     | Toggle to show and hide navigation                                                |
| show\_sharing          | boolean | false    |      |     | Toggle to show and hide sharing                                                   |
| show\_shopspots        | boolean | false    |      |     | Toggle to show and hide sharing in inline tile                                    |
| show\_tags             | boolean | false    |      |     | Toggle to show and hide tags                                                      |
| show\_votes            | boolean | false    |      |     | Toggle to show and hide votes                                                     |

#### `config.claim_config` properties

| Field                 | Type    | Value | POST | PUT | Definition                             |
| --------------------- | ------- | ----- | ---- | --- | -------------------------------------- |
| show\_claim\_button   | boolean | false |      |     | toggle to show and hide claim button   |
| show\_content\_toggle | boolean | false |      |     | toggle to show and hide content toggle |

[Back to Top](style-and-config-properties.md#top)

## Slideshow (base\_slideshow)

### `style` Properties

Number of columns for tile    Name of widget

| Field                                 | Type    | Value                                          | POST | PUT | Definition                                   |
| ------------------------------------- | ------- | ---------------------------------------------- | ---- | --- | -------------------------------------------- |
| arrow\_color                          | string  | ffffff                                         |      |     | Hex code of color                            |
| arrow\_foreground                     | string  | 333333                                         |      |     | Hex code of color                            |
| auto\_scroll                          | boolean | true                                           |      |     | Flag to enable auto scroll                   |
| click\_through\_url                   | string  | \[NONE]                                        |      |     | Click through URL                            |
| columns                               | 1       |                                                |      |     |                                              |
| height                                | integer | 640                                            |      |     | Widget height                                |
| image\_tile\_background               | string  | ffffff                                         |      |     | Hex code of color                            |
| image\_tile\_font\_color              | string  | ffffff                                         |      |     | Hex code of color                            |
| image\_tile\_font\_size               | integer | 24                                             |      |     | Font size for text                           |
| margin                                | integer | 0                                              |      |     | Tile margin                                  |
| name                                  | string  |                                                |      |     |                                              |
| polling\_frequency                    | integer | 30                                             |      |     | The frequency of polling contents in seconds |
| row                                   | integer | 1                                              |      |     | Number of rows in widget                     |
| style                                 | string  | base\_slideshow                                |      |     | Widget style type.                           |
| text\_tile\_background                | string  | ffffff                                         |      |     | Hex code of color                            |
| text\_tile\_font\_color               | string  | 666666                                         |      |     | Hex code of color                            |
| text\_tile\_font\_size                | integer | 24                                             |      |     | Font size for text                           |
| text\_tile\_user\_handle\_font\_color | string  | 666666                                         |      |     | Hex code of color                            |
| text\_tile\_user\_handle\_font\_size  | integer | 20                                             |      |     | Font size for text                           |
| text\_tile\_user\_name\_font\_color   | string  | 666666                                         |      |     | Hex code of color                            |
| text\_tile\_user\_name\_font\_size    | integer | 24                                             |      |     | Font size for text                           |
| tileHeight                            | integer | 640                                            |      |     | Tile height                                  |
| tileWidth                             | integer | 640                                            |      |     | Tile width                                   |
| tiles\_per\_page                      | integer | 15                                             |      |     | Number of tile per page load                 |
| type                                  | string  | fixed                                          |      |     |                                              |
| widget\_background                    | string  | ffffff                                         |      |     | Hex code of color                            |
| widget\_loading\_image                | string  | http://sdp-media.dev/images/widget/loading.gif |      |     | URL of the widget loading image              |
| width                                 | integer | 640                                            |      |     | Widget width                                 |

[Back to Top](style-and-config-properties.md#widget-style-properties)

## Billboard (base\_billboard)

### `style` Properties

Hex code of color

| Field                                 | Type    | Value                                          | POST | PUT | Definition                                   |
| ------------------------------------- | ------- | ---------------------------------------------- | ---- | --- | -------------------------------------------- |
| arrow\_color                          | string  | ffffff                                         |      |     | Hex code of color                            |
| click\_through\_url                   | string  | \[NONE]                                        |      |     | Click through URL                            |
| columns                               | integer | 3                                              |      |     | Number of columns for tile                   |
| height                                | integer | 300                                            |      |     | Widget height                                |
| image\_tile\_background               | string  | ffffff                                         |      |     | Hex code of color                            |
| image\_tile\_font\_color              | string  | ffffff                                         |      |     | Hex code of color                            |
| image\_tile\_font\_size               | integer | 17                                             |      |     | Font size for text                           |
| margin                                | integer | 7                                              |      |     | Tile margin                                  |
| name                                  | string  | Billboard                                      |      |     | Name of widget                               |
| navigation                            | string  | click                                          |      |     |                                              |
| polling\_frequency                    | integer | 30                                             |      |     | The frequency of polling contents in seconds |
| row                                   | integer | 10                                             |      |     | Number of rows in widget                     |
| style                                 | string  | base\_billboard                                |      |     | Widget style type.                           |
| text\_tile\_background                | string  | ffffff                                         |      |     | Hex code of color                            |
| text\_tile\_font\_color               | string  | 666666                                         |      |     | Hex code of color                            |
| text\_tile\_font\_size                | integer | 17                                             |      |     | Font size for text                           |
| text\_tile\_user\_handle\_font\_color | string  | 666666                                         |      |     | Hex code of color                            |
| text\_tile\_user\_handle\_font\_size  | integer | 13                                             |      |     | Font size for text                           |
| text\_tile\_user\_name\_font\_color   | string  | 666666                                         |      |     | Hex code of color                            |
| text\_tile\_user\_name\_font\_size    | integer | 16                                             |      |     | Font size for text                           |
| tileHeight                            | integer | 300                                            |      |     | Tile height                                  |
| tileWidth                             | integer | 300                                            |      |     | Tile width                                   |
| tiles\_per\_page                      | integer | 9                                              |      |     | Number of tile per page load                 |
| type                                  | string  | fixed                                          |      |     |                                              |
| widget\_background                    | string  |                                                |      |     |                                              |
| widget\_loading\_image                | string  | http://sdp-media.dev/images/widget/loading.gif |      |     | URL of the widget loading image              |
| width                                 | integer | 970                                            |      |     | Widget width                                 |

[Back to Top](style-and-config-properties.md#top)

## Feed (base\_feed)

### `style` Properties

<table><thead><tr><th width="253">Field</th><th>Type</th><th>Value</th><th>POST</th><th>PUT</th><th>Definition</th></tr></thead><tbody><tr><td>arrow_color</td><td>string</td><td>000000</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>click_through_url</td><td>string</td><td>[NONE]</td><td></td><td></td><td>Click through URL</td></tr><tr><td>columns</td><td>integer</td><td>3</td><td></td><td></td><td>Number of columns for tile</td></tr><tr><td>height</td><td>integer</td><td>600</td><td></td><td></td><td>Widget height</td></tr><tr><td>image_tile_background</td><td>string</td><td>ffffff</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>image_tile_font_color</td><td>string</td><td>666666</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>image_tile_font_size</td><td>integer</td><td>18</td><td></td><td></td><td>Font size for text</td></tr><tr><td>margin</td><td>integer</td><td>15</td><td></td><td></td><td>Tile margin</td></tr><tr><td>name</td><td>string</td><td>Feed</td><td></td><td></td><td>Name of widget</td></tr><tr><td>navigation</td><td>string</td><td>click</td><td></td><td></td><td></td></tr><tr><td>polling_frequency</td><td>integer</td><td>30</td><td></td><td></td><td>The frequency of polling contents in seconds</td></tr><tr><td>row</td><td>integer</td><td>3</td><td></td><td></td><td>Number of rows in widget</td></tr><tr><td>style</td><td>string</td><td>base_feed</td><td></td><td></td><td>Widget style type.</td></tr><tr><td>text_tile_background</td><td>string</td><td>ffffff</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>text_tile_font_color</td><td>string</td><td>666666</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>text_tile_font_size</td><td>integer</td><td>18</td><td></td><td></td><td>Font size for text</td></tr><tr><td>text_tile_user_handle_font_color</td><td>string</td><td>666666</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>text_tile_user_handle_font_size</td><td>integer</td><td>13</td><td></td><td></td><td>Font size for text</td></tr><tr><td>text_tile_user_name_font_color</td><td>string</td><td>666666</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>text_tile_user_name_font_size</td><td>integer</td><td>16</td><td></td><td></td><td>Font size for text</td></tr><tr><td>tileHeight</td><td>integer</td><td>253</td><td></td><td></td><td>Tile height</td></tr><tr><td>tileWidth</td><td>integer</td><td>253</td><td></td><td></td><td>Tile width</td></tr><tr><td>tiles_per_page</td><td>integer</td><td>9</td><td></td><td></td><td>Number of tile per page load</td></tr><tr><td>type</td><td>string</td><td>fixed</td><td></td><td></td><td></td></tr><tr><td>widget_background</td><td>string</td><td>ffffff</td><td></td><td></td><td>Hex code of color</td></tr><tr><td>widget_loading_image</td><td>string</td><td>http://sdp-media.dev/images/widget/loading.gif</td><td></td><td></td><td>URL of the widget loading image</td></tr><tr><td>width</td><td>integer</td><td>800</td><td></td><td></td><td>Widget width</td></tr></tbody></table>

[Back to Top](style-and-config-properties.md#top)

## Blank Canvas (base\_blankcanvas)

### `style` Properties

| Field            | Type    | Value             | POST | PUT | Definition                   |
| ---------------- | ------- | ----------------- | ---- | --- | ---------------------------- |
| name             | string  | Blank Canvas      |      |     | Name of widget               |
| style            | string  | base\_blankcanvas |      |     | Widget style type.           |
| tiles\_per\_page | integer | 30                |      |     | Number of tile per page load |
| type             | string  | fluid             |      |     |                              |

[Back to Top](style-and-config-properties.md#top)

## Map Leaflet (base\_map\_leaflet)

### `style` Properties

| Field         | Type    | Value                | POST | PUT | Definition         |
| ------------- | ------- | -------------------- | ---- | --- | ------------------ |
| defaultZoom   | integer | 1                    |      |     |                    |
| center        | string  | -33.8,151.2          |      |     |                    |
| minZoom       | integer | 1                    |      |     |                    |
| maxZoom       | integer | 19                   |      |     |                    |
| name          | string  | Map                  |      |     | Name of widget     |
| providerLayer | string  | OpenStreetMap.Mapnik |      |     |                    |
| pinColor      | string  | b53f35               |      |     | Hex code of color  |
| pinTextColor  | string  | ffffff               |      |     | Hex code of color  |
| style         | string  | base\_map\_leaflet   |      |     | Widget style type. |
| type          | string  | fluid                |      |     |                    |

[Back to Top](style-and-config-properties.md#top)
