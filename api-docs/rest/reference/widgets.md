# Widgets API

* [Properties](widgets.md#widgets-properties)
  * [`style` properties](widgets.md#widget-style-properties)
  * [`config` properties](widgets.md#widget-config-properties)
* [GET /api/widgets](widgets.md#GET-/api/widgets)
* [POST /api/widgets](widgets.md#POST-/api/widgets)
* [GET /api/widgets/:widgetId](widgets.md#GET-/api/widgets/widgetId)
* [PUT /api/widgets/:widgetId](widgets.md#PUT-/api/widgets/widgetId)
* [DELETE /api/widgets/:widgetId](widgets.md#DELETE-/api/widgets/widgetId)

## Widgets

Widgets refer to any touchpoint that can be embedded onto a digital property that is made up of either an "In-Line" or possibly also an "Expanded Tile" view. This endpoint allows you to create widgets and returns an embed code.

### Properties

| Field                 | Type                                          | POST  | PUT   | Definition                                  |
| --------------------- | --------------------------------------------- | ----- | ----- | ------------------------------------------- |
| id                    | integer                                       | **X** | **X** | Widget ID                                   |
| widget\_type\_id      | integer                                       | **X** | **X** | Widget type id                              |
| stack\_id             | integer                                       | **X** | **X** | Widget stack Id                             |
| guid                  | string                                        | **X** | **X** | Widget GUID                                 |
| style                 | [object](widgets.md#widget-style-properties)  | ✔     | ✔     | Widget style config                         |
| config                | [object](widgets.md#widget-config-properties) | ✔     | ✔     | Widget config                               |
| filter\_id            | integer                                       | ✔     | ✔     | Content filter for widget                   |
| enabled               | boolean                                       | ✔     | ✔     | Enabled widget                              |
| custom\_css           | string                                        | ✔     | ✔     | Custom css for inline tile                  |
| lightbox\_custom\_css | string                                        | ✔     | ✔     | Custom css for lightbox                     |
| custom\_js            | string                                        | ✔     | ✔     | Custom javascript for inline tile           |
| lightbox\_custom\_js  | string                                        | ✔     | ✔     | Custom javascript for lightbox              |
| external\_js          | string                                        | ✔     | ✔     | URL of external js to inject to parent page |
| embed\_code           | string                                        | **X** | **X** | The widget embed code                       |
| gen                   | integer                                       | **X** | **X** | Version of widget                           |
| style\_name           | string                                        | **X** | **X** |                                             |
| created               | string                                        | **X** | **X** | ISO Date Time string                        |
| modified              | string                                        | **X** | **X** | ISO Date Time string                        |
| created\_at           | timestamp                                     | **X** | **X** | Moderationview's creation time              |
| updated\_at           | timestamp                                     | **X** | **X** | Moderationview's modification time          |

#### `style` properties

Widget style:

* [Gallery (base\_gallery)](style-and-config-properties.md#widgets-style-base_gallery)
* [Waterfall (base\_waterfall)](style-and-config-properties.md#widgets-style-base_waterfall)
* [Carousel (base\_carousel)](style-and-config-properties.md#widgets-style-base_carousel)
* [Slideshow (base\_slideshow)](style-and-config-properties.md#widgets-style-base_slideshow)
* [Billboard (base\_billboard)](style-and-config-properties.md#widgets-style-base_billboard)
* [Feed (base\_feed)](style-and-config-properties.md#widgets-style-base_feed)
* [Blank Canvas (base\_blankcanvas)](style-and-config-properties.md#widgets-style-base_blankcanvas)
* [Map Leaflet (base\_map\_leaflet)](style-and-config-properties.md#widgets-style-base_map_leaflet)

#### `config` properties

* [Gallery (base\_gallery)](style-and-config-properties.md#widgets-config-base_gallery)
* [Waterfall (base\_waterfall)](style-and-config-properties.md#widgets-config-base_waterfall)
* [Carousel (base\_carousel)](style-and-config-properties.md#widgets-config-base_carousel)

[Back to Top](widgets.md#top)

### GET widgets

Retrieves all widget available in the Stack.

#### Resource URL

`https://api.stackla.com/api/widgets`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

No additional request parameters are available.

[Back to Top](widgets.md#top)

### POST widgets

Creates a new widget in the Stack.

#### Resource URL

`https://api.stackla.com/api/widgets`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

No additional request parameters are available.

[Back to Top](widgets.md#top)

### GET widgets/:widgetId

Retrieves a specific widget available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/widgets/:widgetId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name     | Mandatory | Request type | Description      |
| -------- | --------- | ------------ | ---------------- |
| widgetId | Yes       | endpoint     | ID of the widget |

[Back to Top](widgets.md#top)

### PUT widgets/:widgetId

Updates a specific widget available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/widgets/:widgetId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name     | Mandatory | Request type | Description      |
| -------- | --------- | ------------ | ---------------- |
| widgetId | Yes       | endpoint     | ID of the widget |

[Back to Top](widgets.md#top)

### DELETE widgets/:widgetId

Deletes a specific widget available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/widgets/:widgetId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name     | Mandatory | Request type | Description      |
| -------- | --------- | ------------ | ---------------- |
| widgetId | Yes       | endpoint     | ID of the widget |

[Back to Top](widgets.md#top)
