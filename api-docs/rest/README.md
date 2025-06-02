# REST API

To interface with some core UGC functionality, Nosto has exposed specific resources via a [REST API](http://en.wikipedia.org/wiki/Representational_state_transfer). This gives developers a great deal of flexibility when integrating Nosto and opens up the doors to very exciting activations. While the JavaScript API deals mainly with the display of content, the REST API provides a way for the backend to interface directly with the Nosto configuration, data, and functionality. Check out [our guides](../../guides/rest-api/) to see a sample of use cases.

## Usage

Nosto's UGC REST API is the key to the integration point of many backend client applications, used to manage all areas of the Nosto product, enrich data stores, and ensure content taxonomy is updated in real-time.

**Key Features**

* Rate limited.
* Prioritises real-time uncached data results.
* Powerful control set to match core Nosto's UGC functionality.
* Secure access via OAuth tokens.
* Confirms REST structure and HTTP messaging.
* Restricts browser-based requests.

**Designed for**

* Powering custom B2B applications.
* Integrating with CRM and BI tooling.
* Managing real-time data via external management tools.
* Enriching external data stores.
* Synching external content taxonomy.

### Rate Limits

The API enforces a simple rate limit of 450 requests per 15-minute (900-second) time window per client. This includes all types of requests, namely GET, POST, PUT, and DELETE. Requests made in excess will be blocked and return an HTTP 403 status (Rate Limit Exceeded). Clients who exceed this rate limit constantly may be blocked by API access revocation, IP address, or otherwise (see [Terms of Use](https://github.com/Stackla/docs/blob/master/terms-of-use/README.md)).

To measure current use and limits, you can either refer to your Nosto's UGC API Console, or by inspecting the response headers of every request:

* **X-API-Remaining-Requests-Reset** - The time (in seconds) remaining until the Rate Limit Time Window resets
* **X-API-Remaining-Requests** - The number of requests available in the current Rate Limit Time Window

**Note about measuring rate usage**

There are up to 2 additional requests reserved in the rate limit time window for querying a future (i.e. to be implemented) API resource. The data returned in the header will reflect this.

Most uses of the API will easily fit inside this rate limit by utilizing server-side caching mechanisms. For more information about this, please reach out to our [support team](https://github.com/Stackla/docs/blob/master/support/README.md). If you believe that you need to increase your rate limit allowance, please contact your sales representative.

### Authentication and Authorisation

The Nosto's UGC REST API supports basic authentication and authorization as an OAuth2 provider.

Each use of the REST API requires a valid client token, provided by the _access\_token_ URL query parameter or the _x-access-token_ header parameter. Each client token will perform actions on behalf of the user that has authorized its use.

Generating a client token can be done either in the Admin Portal (as each admin, individually) or by implementing the authorization protocol of the REST API application yourself. You will most likely perform the former, however, if you need to implement it yourself, our [SDK](https://github.com/Stackla/docs/blob/master/api-docs/sdk/README.md) may be of use to get you started.

#### How to Generate an Client Token via the Admin Portal&#x20;

Follow these steps to generate your API key:

1. **Log in to your Nosto account**
2. **Go to Tools > REST API Console.**\
   From the left-hand navigation menu, select **Tools**, then click on **REST API Console**.
3. **Generate an Access Token.**\
   Scroll to the **Access Token** section and click **Generate Token**.\
   Copy and store the generated token securely — it will be required for authenticating your API requests.

> ⚠️ Keep your access token secure. Do not share it or expose it in client-side code.

<figure><img src="../../.gitbook/assets/Screenshot 2025-06-02 at 5.56.40 pm.png" alt=""><figcaption></figcaption></figure>

#### How to find your REST API Details )&#x20;

### Content Type and Accept Headers

All POST requests to the API must be made with Content-Type set to "multipart/form-data". At this point, the API will not interpret any other Content-Types. Other request types should specify pointers or operators in URL-encoded queries.

Requests can be made to respond in both JSON ("application/json") or XML ("application/xml") format. To modify the accepted type, you may specify it in either the HTTP header or the _Accept_ header (e.g. "Accept: application/xml"). You can also specify it as the resource extension. For example, to request Filter ID 123 to be returned as XML format, your request may resemble:

https://api.stackla.com/api/filters/123\*\*.xml\*\*?access\_token=1234abcd5678efgh\&stack=mystack

Similarly, change the XML above to JSON to have it respond in JSON format.

If no type is specified, JSON is assumed and preferred over XML. Please note that the XML format is a limited implementation and provided on an "as-is" basis, and may only be suitable for some implementations.

### Response Format

All responses will contain a specific HTTP status and wrap the payload in _data_ and _error_ branches/fields. _Data_ may be an object, or an array of objects, depending on the request. _Errors_ will either be an empty array or an array of error codes and messages.

**Example of a valid response, with no errors (formatted for clarity)**

```
{
    "data": {
        "id": "1234",
        "stack_id": "9999",
        "name": "Latest",
        ...
    },
    "errors": []
}
```

**Example of an error (formatted for clarity)**

Note: `message` is a general description of the reason for the error, it sometimes can be an object.

Note: `error_code` and `message_id` are 2 new fields that contain unique values for different types of errors. Due to the amount of work, this is still in progress. If an error has not been defined, `error_code` returns 0.

```
{
    "data": [],
    "errors": [
        {
            "message": "Invalid resource specified or resource not found",
            "code": 404,
            "error_code": 1080404,
            "message_id": "term:not_found"
        }
    ]
}
```

#### Result Counters

Some queries (such as Tag keyword searching/matching) also return counters based on totals and how many the query matched. For example, if you have a total of 200 tags, a search for the keyword "%hello%" yields 150 results, you should see something like the following.

**Note:** These numbers are not necessarily related to the results returned and their counters.

```
{
    "data": [
        ...
    ],
    "errors": [],
    "count": {
        "results_total": "150",
        "total": "200"
    }
}
```

### Pagination

Results sets can be navigated in sets of _pages_ of a finite limit. The limit of each page can be set via the _limit_ parameter, and can range between 1 and 100 results. Both the page and limit parameters can be set as a URL query, such as:

https://api.stackla.com/api/filters.json?access\_token=1234abcd5678efgh\&stack=mystack\*\*\&limit=10\&page=3\*\*

In this example (i.e. page 3 and page limit to 10), the request would yield results 21 to 30.

### Status and Error Codes

Every request to the interface will result in one of the following codes. You may adjust your application strategy to deal with the HTTP responses (e.g. "back-off" when rate limit is exceeded, trigger alert when a 400 or 401 is returned, etc.).

| Status                             | HTTP Code | Description                                                                                                     |
| ---------------------------------- | --------- | --------------------------------------------------------------------------------------------------------------- |
| API\_STATUS\_OK                    | 200       | OK: All is good, the request was received, and processed successfully and data was returned.                    |
| API\_STATUS\_BAD\_REQUEST          | 400       | Bad request: The request could not be understood                                                                |
| API\_STATUS\_UNAUTHORIZED          | 401       | Unauthorized: Authentication credentials invalid or not authorised to access resource                           |
| API\_STATUS\_RESTRICTED            | 403       | Forbidden: Access to resource is restricted                                                                     |
| API\_STATUS\_RATE\_LIMIT\_EXCEEDED | 429       | Rate limit exceeded: Too many requests in the current time window                                               |
| API\_STATUS\_INVALID\_RESOURCE     | 404       | Invalid resource: Invalid resource specified or resource not found                                              |
| API\_STATUS\_SERVER\_ERROR         | 500       | Application error: An error within the application has prohibited a successful response; please contact support |
| (An empty status will be returned) | 503       | Network error: An error within the network prohibited a successful response; please contact support             |
| (An empty status will be returned) | 504       | Network error: An error within the network prohibited a successful response; please contact support             |

There may be occasions where other error codes (such as 5xx status) are returned. In this case please contact Support to investigate.
