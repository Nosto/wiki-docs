# Tiles API

* [Properties](tiles.md#tiles-properties)
* [GET /api/tiles/:tileId](tiles.md#GET-/api/tiles/tileId)
* [POST /api/tiles](tiles.md#POST-/api/tiles)
* [PUT /api/tiles/:tileId](tiles.md#PUT-/api/tiles/tileId)

## Tiles

The Tile is defined as any post/comment stored within Nosto's UGC that has been ingested from an external source (ie. Tweet from Twitter, Post from FB)

### Properties

<table data-full-width="true"><thead><tr><th>Field</th><th>Type</th><th>Supported Values</th><th>POST</th><th>PUT</th><th>Definition</th></tr></thead><tbody><tr><td>_id</td><td>objectId</td><td>-</td><td><strong>X</strong></td><td><strong>X</strong></td><td>Unique identifier for the Tile, in the Stack. This is an object containing a “$id” property, which will expose the ID as a 24-byte string.<br><br><strong>Example Values</strong>:<br><code>550f9f0e29115c0000bf82cb</code></td></tr><tr><td>message</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>post: <strong>Required</strong></p><p></p><p>Message body, normalised from the content source. Will be the Tweet text, Facebook status, Instagram caption, etc. Maximum 32k characters.</p></td></tr><tr><td>title</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td>Tile title to accompany the message, often used for video tiles.<br><br><strong>Example Values</strong>: <code>Wow, this is such an old photo! #tbt</code></td></tr><tr><td>html</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>HTML body, up to 32k characters. This field is mandatory for "html" media type.<br></p><p><strong>Example Values</strong>: <code>Dog on a surfboard</code></p></td></tr><tr><td>media</td><td>enum</td><td><code>text</code><br><code>image</code><br><code>video</code><br><code>html</code></td><td>✔</td><td><strong>X</strong></td><td><p>The media type of the post.<br></p><p><strong>Example Values</strong>: <code>text</code>, <code>image</code></p></td></tr><tr><td>image_url</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td>Full-sized image URL.<br><br><strong>Example Values</strong>: <code>https://stackla.com/images/buddy_the_dog.jpg</code></td></tr><tr><td>image_small_url</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Small image URL (ideally under to 300x300px, or 600x600px for retina).<br></p><p><strong>Example Values</strong>: <code>https://stackla.com/images/buddy_the_dog-600x600.jpg</code></p></td></tr><tr><td>image_medium_url </td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td>Medium image URL (ideally under to 600x600px, or 1200x1200px for retina).<br><br><strong>Example Values</strong>: <code>https://stackla.com/images/buddy_the_dog-1200x1200.jpg</code></td></tr><tr><td>tags</td><td>array</td><td>-</td><td>✔</td><td>Yes</td><td><p>Array of Tag IDs associated with the Tile. Note: May be returned as array of Integers or Strings.<br></p><p><strong>Example Values</strong>: <code>["14001","14562"]</code></p></td></tr><tr><td>share_text</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Accompanying text to be used when tile is shared on a Social network (e.g. Twitter, Instagram, Facebook, etc.).<br></p><p><strong>Example Values</strong>: <code>Come and see Jimmy's post on the stack!</code></p></td></tr><tr><td>video_url</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>URL of the video file. Required when media type is "video").<br></p><p><strong>Example Values</strong>: <code>https://stackla.com/images/dog_on_a_surfboard.mp4</code></p></td></tr><tr><td>width_ratio</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Tile width ratio to be used as a ratio vs width_ratio as width to height. Positive numeric value. Required when media type is "html".<br></p><p><strong>Example Values</strong>: <code>3</code></p></td></tr><tr><td>height_ratio</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Tile width ratio to be used as a ratio vs height_ratio as width to height. Positive numeric value. Required when media type is "html".<br></p><p><strong>Example Values</strong>: <code>4</code></p></td></tr><tr><td>status</td><td>string</td><td><code>published</code><br><code>queued</code><br><code>disabled</code></td><td>✔</td><td>✔</td><td><p>Tile moderation status.<br></p><p><strong>Example Values</strong>: <code>published</code></p></td></tr><tr><td>avatar</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>URL to be used as the post author's avatar.<br></p><p><strong>Example Values</strong>: <code>https://stackla.com/images/mascots/buddy.jpg</code></p></td></tr><tr><td>name</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Display name to be used for the author.<br></p><p><strong>Example Values</strong>: <code>Fred Flintstone</code></p></td></tr><tr><td>guid</td><td>string</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Globally unique ID to be used for this post (often referencing external ID). Tiles that have this value set can also be retrived using the <code>guid:</code> operator (see <a href="tiles.md#GET-/api/tiles/tileId-_id">_id</a>). For more info see the <a href="tiles.md#POST-/api/tiles-guid">POST tiles guid reference</a>.<br></p><p><strong>Example Values</strong>: <code>2019</code></p></td></tr><tr><td>anonymous</td><td>bool</td><td>-</td><td>✔</td><td><strong>X</strong></td><td><p>Tiles that originate from networks that allow anonymous posts (including Nost's UGC) will have this field set to <code>true</code>. Renderers should not show any attributions the creator of this Tile.<br></p><p><strong>Example Values</strong>: <code>false</code></p></td></tr><tr><td>longitude</td><td>string</td><td>-</td><td>✔</td><td>✔</td><td><p>GPS longitude co-ordinate for geo-location<br></p><p><strong>Example Values</strong>: <code>144.953151388</code></p></td></tr><tr><td>latitude</td><td>string</td><td>-</td><td>✔</td><td>✔</td><td><p>GPS latitude co-ordinate for geo-location<br></p><p><strong>Example Values</strong>: <code>-37.816644361</code></p></td></tr><tr><td>source</td><td>string</td><td><code>twitter</code><br><code>facebook</code><br><code>instagram</code><br><code>youtube</code><br><code>gplus</code><br><code>flickr</code><br><code>pinterest</code><br><code>tumblr</code><br><code>rss</code><br><code>ecal</code><br><code>sta_feed</code><br><code>weibo</code></td><td><strong>X</strong></td><td><strong>X</strong></td><td><p>The source of the post, often a social network. This field is also referred to as "network" in the filter context</p><p><strong>Example Values</strong>: <code>twitter</code></p></td></tr></tbody></table>

### GET tiles/:tileId

Retrieves a specific Tile available in the Stack by its ID or GUID.

#### Resource URL

`https://api.stackla.com/api/tiles/:tileId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>tileId</td><td>Yes</td><td>endpoint</td><td>ID of the Tile, or its Globally Unique ID (GUID, prefixed with "guid:").<br><br><strong>Example Values</strong>: <code>550f9f0e29115c0000bf82cb</code>, <code>guid:1234</code></td></tr></tbody></table>

[Back to Top](tiles.md#top)

### POST tiles

Creates a new Tile in the Stack.

#### Resource URL

`https://api.stackla.com/api/tiles?term_id=:termId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User, Stack

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Field</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>termId</td><td>No</td><td>query</td><td>ID of the UGC Post Feed term that this content will be ingested by.<br><br><strong>Example Values</strong>: <code>9999</code></td></tr></tbody></table>

[Back to Top](tiles.md#top)

### PUT tiles/:tileId

Updates a specific Tile available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/tiles/:tileId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User, Stack

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>tileId</td><td>Yes</td><td>endpoint</td><td>ID of the Tile, or its Globally Unique ID (GUID, prefixed with "guid:").<br><br><strong>Example Values</strong>: <code>550f9f0e29115c0000bf82cb</code>, <code>guid:12345</code></td></tr></tbody></table>

[Back to Top](tiles.md#top)
