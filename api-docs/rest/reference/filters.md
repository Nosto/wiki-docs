# Filters API

* [Properties](filters.md#filters-properties)
* [GET /api/filters](filters.md#GET-/api/filters)
* [POST /api/filters](filters.md#POST-/api/filters)
* [GET /api/filters/:filterId](filters.md#GET-/api/filters/filterId)
* [GET /api/filters/:filterId/content](filters.md#GET-/api/filters/filterId/content)
* [GET /api/filters/:filterId/summaries](filters.md#GET-/api/filters/filterId/summaries)
* [PUT /api/filters/:filterId](filters.md#PUT-/api/filters/filterId)
* [DELETE /api/filters/:filterId](filters.md#DELETE-/api/filters/filterId)

## Filters

A Filter is a query for UGC Tile content that was ingested from a particular Network, contains a particular Tag, and/or is of a particular Medium. To retrieve a stream of content you can do so using the Filters endpoint.

### Properties

<table data-full-width="true"><thead><tr><th>Field</th><th>Type</th><th>Supported Values</th><th>POST</th><th>PUT</th><th>Definition</th></tr></thead><tbody><tr><td>id</td><td>integer</td><td>-</td><td>✔</td><td><strong>X</strong></td><td>Unique id for filter</td></tr><tr><td>name</td><td>string</td><td>-</td><td>✔</td><td>✔</td><td>post: <strong>Required</strong><br><br>Filter name</td></tr><tr><td>sort</td><td>string</td><td><code>source_created_at_desc</code><br><code>score_desc</code><br><code>votes</code></td><td>✔</td><td>✔</td><td>Sorting type</td></tr><tr><td>networks</td><td>array</td><td><code>twitter</code><br><code>facebook</code><br><code>instagram</code><br><code>youtube</code><br><code>gplus</code><br><code>flickr</code><br><code>pinterest</code><br><code>tumblr</code><br><code>rss</code><br><code>ecal</code><br><code>sta_feed</code><br><code>weibo</code></td><td>✔</td><td>✔</td><td>array of network(s) for filtering rule</td></tr><tr><td>tags</td><td>array</td><td>-</td><td>✔</td><td>✔</td><td>Array of tags' id for filtering rule</td></tr><tr><td>media</td><td>array</td><td><code>text</code><br><code>image</code><br><code>video</code><br><code>html</code></td><td>✔</td><td>✔</td><td>Array of media(s) for filtering rule</td></tr></tbody></table>

[Back to Top](filters.md#top)

### GET filters

Retrieves all filters available in the Stack.

#### Resource URL

`https://api.stackla.com/api/filters`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>page</td><td>No</td><td>query</td><td>Page number. Default value is 1</td></tr><tr><td>limit</td><td>No</td><td>query</td><td>Return limit define how many Filters will be return for each request. Default is 25. Maximum limit is 100.</td></tr></tbody></table>

[Back to Top](filters.md#top)

### POST filters

Creates a new Filter in the Stack.

#### Resource URL

`https://api.stackla.com/api/filters`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

No additional request parameters are available.

[Back to Top](filters.md#top)

### GET filters/:filterId

Retrieves a specific Filter available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/filters/:filterId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>filterId</td><td>Yes</td><td>Request type</td><td>ID of the Filter</td></tr></tbody></table>

[Back to Top](filters.md#top)

### GET filters/:filterId/content

Retrieves tile content of a specific Filter by its ID.

#### Resource URL

`https://api.stackla.com/api/filters/:filterId/content`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>filterId</td><td>Yes</td><td>endpoint</td><td>ID of the Filter</td></tr><tr><td>since_id</td><td>No</td><td>query</td><td>Return results with an ID greater than (more recent than) the specified ID</td></tr><tr><td>status</td><td>No</td><td>query</td><td><p>Returns tiles with a moderation status. By default, is field is not set only Published tiles are returned<br></p><p><strong>Example Values</strong>: <code>published, queued, disabled</code></p></td></tr></tbody></table>

[Back to Top](filters.md#top)

### GET filters/:filterId/summaries

Retrieves aggregated data of a specific Filter by its ID.

#### Resource URL

`https://api.stackla.com/api/filters/:filterId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>filterId</td><td>Yes</td><td>endpoint</td><td>ID of the Filter</td></tr><tr><td>aggregated_by</td><td>No</td><td>query</td><td>Default or geohash (beta), geohash returns aggregated data grouped by geohashes for geotagged tiles.</td></tr></tbody></table>

[Back to Top](filters.md#top)

### PUT filters/:filterId

Updates a specific Filter available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/filters/:filterId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>filterId</td><td>Yes</td><td>endpoint</td><td>ID of the Filter</td></tr></tbody></table>

[Back to Top](filters.md#top)

### DELETE filters/:filterId

Deletes a specific Filter available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/filters/:filterId`

#### Resource Details

**Rate limited:** Yes

**Access scope:** User

#### Request Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Mandatory</th><th>Request type</th><th>Description</th></tr></thead><tbody><tr><td>filterId</td><td>Yes</td><td>endpoint</td><td>ID of the Filter</td></tr></tbody></table>

[Back to Top](filters.md#top)
