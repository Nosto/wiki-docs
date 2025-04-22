# Reference

## Finding your plugin's Unique identifier (Plugin ID)

The identifier `{Plugin ID}`and API Endpoint are generated when the Content API is configured in the Nosto Admin Portal.

1. Login to the Nosto Admin Portal > UGC.
2. On the sidebar, click Tools > Content API.

## Content filters

By default, the Content API can be configured to output content from a specific filter. This is configured within the admin portal.

To output content from all filters, you can set the configured filter to `latest` (assuming this filter hasn't been customized). In this configuration, the Content API can access any content within a Stack and, using tags, can target specific items or groups of items.

Dynamically setting the override filter to the ID of the `latest` filter when the `tags` parameter is used is a helpful method to ensure _any_ content with those tags is returned, not just the content from the configured filter.

Your Stack's filter IDs can be found on the filter's page in your Nosto Admin Portal. Click Curate > Filters on the side navigation bar.

## Security

### Access control

Each Content API has a unique identifier, making each URL private by default.

Nosto's UGC Content API is designed to be open to access and does not implement any access control mechanisms. Sharing the private URL, making it publicly known, or using it within Javascript on a website will allow all the below requests to be made by anybody. Keep this in mind when designing integrations with this plugin.

**NOTE:** To generate a new unique identifier, the plugin will have to be reinstalled from within the Nosto admin portal.

### Allowed domains

When making requests to the API in a browser via AJAX, a pre-flight HTTP OPTIONS request will be made to validate the Cross-Origin Resource Sharing (CORS) policy.

By default, the `Access-Control-Allow-Origin` response will allow all domains. e.g. `*`

A comma-separated list of allowed domains can be configured in the plugin configuration page. Adding domains to this list will change the API's response to any pre-flight HTTP OPTIONS requests made by the browser to contain only the domain(s) specified.

**NOTE:** This does not restrict access to the Content API, or its output. It simply limits its usage from within unauthorized websites using modern browsers' built-in protections.

Read more about CORS [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

## Example Requests

The following example requests illustrate some common use cases of the Content API, using the available request parameters.

### Content Type

The Content API outputs data in the JSON format. Valid responses containing JSON will always include the HTTP header `Content-Type: application/json`. This should be used to validate the output of the Content API in case of an unexpected error (see below).

### Available request parameters

| Name           | Expected Value                       |
| -------------- | ------------------------------------ |
| **limit**      | Integer                              |
| **page**       | Integer                              |
| **filter\_id** | Integer                              |
| **tags**       | Comma separated string of tag IDs    |
| **brands**     | Comma separated string of brands     |
| **categories** | Comma separated string of categories |

### Error handling

The Content API is designed to be robust as possible. It does not follow the REST API pattern or utilise HTTP error code conventions to communicate the state of the response.

The following rules are used to handle issues;

**Invalid parameter names or values types will be ignored**

* The request will continue to process.
* No 400 codes or error messages will be returned on invalid requests.

**Valid parameter types with values outside the expected range will default to their \_minimum**\_\*\* values\*\*

* See expected ranges below.

**There is no fallback/default content for filters or tags requests which return empty results**

**Some requests will still return HTTP errors**

* Requests using invalid plugin IDs will return a 404 page.
* Service errors will still return 5xx errors.
* Requests flagged as security violations may return 4xx errors.

### Basic Requests

* Configured filter can default to query all content, using the`latest` filter ID.
* Response data is always cached for **3 minutes** based on query structure.
* Limit range: `1-50` items.
* Page range: `1-999` pages.

#### Default request

Return `50` items from the default filter configured in the admin portal.

`https://plugins.stackla.com/content_api/{Plugin ID}`

#### Limits & Pages

Return `10` items from configured filter.

`https://plugins.stackla.com/content_api/{Plugin ID}?limit=10`

Return `3` items from the second page (items 4, 5 & 6) from configured filter.

`https://plugins.stackla.com/content_api/{Plugin ID}?limit=3&page=2`

### Complex Requests

* Configured filter can default to query all content, using the`latest` filter ID.
* Response data is always cached for **6 minutes** based on query structure.
* Item range: `1-100` items.
* Page range: `1-999` pages.
* Tag limit: `0-20` tag IDs.
* Filter limit: `0-1` filter IDs.

#### Limits & Pages

Return `100` items from configured filter.

`https://plugins.stackla.com/content_api/{Plugin ID}?limit=100`

Return `100` items from configured filter, starting at page `3` (items 300-400).

`https://plugins.stackla.com/content_api/{Plugin ID}?limit=100&page=3`

#### Override Default Filter with Custom Filter

Return `50` items from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123`

Return `75` items from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123&limit=75`

#### Override Default Filter with Custom Filter and tags

Return `50` items matching tags `123`, `456` and `678` from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123&tags=123,456,678`

Return `10` items from the 10th page, matching tags `123`, `456` and `678` from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123&limit=10&page=10&tags=123,456,678`

#### Override Default Filter with Custom Filter and brands

Return `50` items matching brands `nike` from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123&brands=nike`

#### Override Default Filter with Custom Filter and categories

Return `50` items matching categories `shoes` from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123&categories=shoes`

#### External Product tags

**Note**: External tags must use the `ext:` prefix to differentiate them from internal tag IDs. External tag IDs can be strings.

Return `30` items matching product tags using external ID `abc123` from configured filter.

`https://plugins.stackla.com/content_api/{Plugin ID}?limit=30&tags=ext:abc123`

Return `50` items matching product tags using external ID `abc123` and `xyz987` from filter `123`.

`https://plugins.stackla.com/content_api/{Plugin ID}?filter_id=123&tags=ext:abc123,ext:xyz987`
