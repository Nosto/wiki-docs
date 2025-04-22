# Tags API

* [Properties](tags.md#tags-properties)
* [GET /api/tags](tags.md#GET-/api/tags)
* [POST /api/tags](tags.md#POST-/api/tags)
* [GET /api/tags/:tagId](tags.md#GET-/api/tags/tagId)
* [GET /api/tags/:tagId/summaries](tags.md#GET-/api/tags/tagId/summaries)
* [PUT /api/tags/:tagId](tags.md#PUT-/api/tags/tagId)
* [DELETE /api/tags/:tagId](tags.md#DELETE-/api/tags/tagId)

## Tags

Tags allow content to be categorized and filtered for better curation. They are more like blog tags or product swing tags and are not to be confused with hashtags. It is a form of profiling Social Tiles for grouping/association purposes.

### Properties

| Field                 | Type      | Value                                                                                         | POST  | PUT   | Definition                                                                                                                                                                                                                                                                                                       |
| --------------------- | --------- | --------------------------------------------------------------------------------------------- | ----- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                    | integer   |                                                                                               | **X** | **X** | Unique identifier for the Tag. **Example Values**: `12345`                                                                                                                                                                                                                                                       |
| tag                   | tag       |                                                                                               | ✔     | ✔     | <p>post: <strong>Required</strong></p><p>Name and display title on the Tag.</p><p><strong>Example Values</strong>: <code>Editor's choice</code></p>                                                                                                                                                              |
| slug                  | string    |                                                                                               | ✔     | ✔     | <p>post: <strong>Required</strong> if <code>custom_slug</code> is 0</p><p>Simplified and class-name-fiendly Tag identifier, most often auto-generated. On output this value is often used to set the class.</p><p><strong>Example Values</strong>: <code>editors-choice</code></p>                               |
| custom\_slug          | bool      | <p><code>1</code> (true)<br><code>0</code> (false)</p>                                        | ✔     | ✔     | <p>This value specifies if the <code>slug</code> field value is being auto-generated or overwritten by the user.</p><p><strong>Example Values</strong>: <code>0</code></p>                                                                                                                                       |
| type                  | enum      | <p><code>content</code><br><code>product</code><br><code>competition</code></p>               | ✔     | ✔     | <p>post: <strong>Required</strong></p><p>Specifies the type of the Tag</p><p><strong>Example Values</strong>: <code>content</code></p>                                                                                                                                                                           |
| publicly\_visible     | bool      | <p><code>1</code> (true)<br><code>0</code> (false)</p>                                        | ✔     | ✔     | <p>post: <strong>Required</strong></p><p>This value specifies if display renderers should display this Tag in display context</p><p><strong>Example Values</strong>: <code>1</code></p>                                                                                                                          |
| target                | enum      | <p><code>_blank</code><br><code>_self</code><br><code>_parent</code><br><code>_top</code></p> | ✔     | ✔     | <p>When rendered as a link, this will indicate the target attribute for the anchor tag when used with custom_url</p><p><strong>Example Values</strong>: <code>_blank</code></p>                                                                                                                                  |
| system\_tag           | bool      | <p><code>1</code> (true)<br><code>0</code> (false)</p>                                        | **X** | **X** | <p>Indicates whether this Tag is a read-only tag created and managed by the system.</p><p><strong>Example Values</strong>: <code>1</code></p>                                                                                                                                                                    |
| priority              | integer   | 1 (highest) - 5 (lowest)                                                                      | ✔     | ✔     | <p>Specifies the sequential sort order in which this Tag should be displayed when being rendered for display. Default values is 3</p><p><strong>Example Values</strong>: <code>3</code></p>                                                                                                                      |
| custom\_url           | string    |                                                                                               | ✔     | ✔     | <p>URL that clicking on the Tag should take the user to. When type is product, this is the URL that the product click-through should be linked to</p><p><strong>Example Values</strong>: <code>https://www.google.com.au/?q=stackla</code></p>                                                                   |
| price                 | string    |                                                                                               | ✔     | ✔     | <p>User provided price for Tags of type <code>product</code>.</p><p><strong>Example Values</strong>: <code>$399</code></p>                                                                                                                                                                                       |
| ext\_product\_id      | string    |                                                                                               | ✔     | ✔     | <p>User provided reference to external product for Tags of type <code>product</code>. Should be a continous string, best if URL-friendly. When querying for a product by ID, this value can be prefixed with <code>ext:</code> to fetch by it.</p><p><strong>Example Values</strong>: <code>PROD-1234</code></p> |
| description           | string    |                                                                                               | ✔     | ✔     | <p>User provided description for Tags of type product. Maximum length: 512 characters</p><p><strong>Example Values</strong>: <code>Long sleeve woven swing dress in animal print with roll neck and back zip fastening. 87cm length. 100% Viscose. Machine washable.</code></p>                                  |
| image\_small\_url     | string    |                                                                                               | ✔     | ✔     | <p>URL of the small (optimised for 300px x 300px) image PNG/JPG/JPEG/GIF image to be displayed. This should be a HTTPS URL to so that the Stack or widget can be served over HTTPS completely</p><p><strong>Example Values</strong>: <code>https://stackla.com/images/product_small.jpg</code></p>               |
| image\_small\_width   | integer   |                                                                                               | ✔     | ✔     | <p>Width of the small image being used as the image_small_url, in pixels</p><p><strong>Example Values</strong>: <code>250</code></p>                                                                                                                                                                             |
| image\_small\_height  | integer   |                                                                                               | ✔     | ✔     | <p>Height of the small image being used as the image_small_url, in pixels</p><p><strong>Example Values</strong>: <code>200</code></p>                                                                                                                                                                            |
| image\_medium\_url    | string    |                                                                                               | ✔     | ✔     | <p>URL of the medium (optimised for 600px x 600px) image PNG/JPG/JPEG/GIF image to be displayed. This should be a HTTPS URL to so that the Stack or widget can be served over HTTPS completely</p><p><strong>Example Values</strong>: <code>https://stackla.com/images/product_medium.jpg</code></p>             |
| image\_medium\_width  | integer   |                                                                                               | ✔     | ✔     | <p>Width of the small image being used as the image_medium_url, in pixels</p><p><strong>Example Values</strong>: <code>500</code></p>                                                                                                                                                                            |
| image\_medium\_height | integer   |                                                                                               | ✔     | ✔     | <p>Height of the small image being used as the image_medium_url, in pixels</p><p><strong>Example Values</strong>: <code>400</code></p>                                                                                                                                                                           |
| created\_at           | timestamp |                                                                                               | **X** | **X** | <p>UTC timestamp of when this Tag was created</p><p><strong>Example Values</strong>: <code>1372057752</code></p>                                                                                                                                                                                                 |

[Back to Top](tags.md#top)

### GET tags

Retrieves all tags available in the Stack.

#### Resource URL

`https://api.stackla.com/api/tags`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name    | Mandatory | Request type | Description                                                                                                                                                                                   |
| ------- | --------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| keyword | No        | query        | Keyword to search Tag ID, Name, Slug, External URL and External Product ID (prefixed with "ext:"). **Example Values**: `12345`, `ext:PROD-1234`, `my tag`                                     |
| type    | No        | query        | <p>Comma-separated of type of tags to be derived. One or more of content, product, competition, system.</p><p><strong>Example Values</strong>: <code>content</code>, <code>product</code></p> |
| page    | No        | query        | Page number. Default value is 1                                                                                                                                                               |
| limit   | No        | query        | Return limit define how many Tags will be return for each request. Default is 25. Maximum limit is 100.                                                                                       |

[Back to Top](tags.md#top)

### POST tags

Creates a new Tag in the Stack.

#### Resource URL

`https://api.stackla.com/api/tags`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

No additional request parameters are available.

[Back to Top](tags.md#top)

### GET tags/:tagId

Retrieves a specific Tag available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/tags/:tagId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name  | Mandatory | Request type | Description                                                                                                    |
| ----- | --------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| tagId | Yes       | endpoint     | ID of the Tag, or its External Product ID (prefixed with "ext:"). **Example Values**: `12345`, `ext:PROD-1234` |

[Back to Top](tags.md#top)

### GET tags/:tagId/summaries

Retrieves aggregated data of a specific Tags by its ID.

#### Resource URL

`https://api.stackla.com/api/tags/:tagId/summaries`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name  | Mandatory | Request type | Description                                                                                                    |
| ----- | --------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| tagId | Yes       | endpoint     | ID of the Tag, or its External Product ID (prefixed with "ext:"). **Example Values**: `12345`, `ext:PROD-1234` |

[Back to Top](tags.md#top)

### PUT tags/:tagId

Updates a specific Tag available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/tags/:tagId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name  | Mandatory | Request type | Description                                                                                                    |
| ----- | --------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| tagId | Yes       | endpoint     | ID of the Tag, or its External Product ID (prefixed with "ext:"). **Example Values**: `12345`, `ext:PROD-1234` |

[Back to Top](tags.md#top)

### DELETE tags/:tagId

Deletes a specific Tag available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/tags/:tagId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name  | Mandatory | Request type | Description                                                                                                    |
| ----- | --------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| tagId | Yes       | endpoint     | ID of the Tag, or its External Product ID (prefixed with "ext:"). **Example Values**: `12345`, `ext:PROD-1234` |

[Back to Top](tags.md#top)
