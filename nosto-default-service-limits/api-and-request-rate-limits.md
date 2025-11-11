# API and request rate limits

## Nosto API and request rate limits

To ensure our platform remains stable, Nosto’s APIs are rate limited as follows. We ask developers to use industry standard techniques for limiting calls, optimizing requests and platform setups, and re-trying requests responsibly. Customer is subject to the standard limits unless Customer has purchased a Peak Performance package detailing advanced (enterprise or custom) commitments.&#x20;

<table data-header-hidden><thead><tr><th width="176.796875">API and Requests</th><th>Rate-limiting method</th><th width="191.33203125">Standard limit</th><th>Advanced limit</th></tr></thead><tbody><tr><td><strong>API and Requests</strong></td><td><strong>Rate-limiting method</strong></td><td><strong>Standard Limit</strong></td><td><strong>Advanced limit</strong></td></tr><tr><td>Search GraphQL API</td><td>Calculated query cost</td><td>10.000 points/second</td><td>50.000 points/second</td></tr><tr><td>Recommendations Requests</td><td>Calculated query cost</td><td>10.000 points/second</td><td>80.000 points/second</td></tr><tr><td>GraphQL API</td><td>Calculated query cost</td><td>10.000 points/second</td><td>50.000 points/second</td></tr></tbody></table>

Nosto may temporarily reduce API rate limits to protect platform stability. We will strive to keep these instances brief and rare. However, your application should be built to handle limits gracefully.

#### Rate limiting methods

Nosto's API's use a calculated query cost method.&#x20;

Merchants can make requests that cost a maximum number of **points** each second. For example, 1000 points within 1 seconds. More complex requests cost more points, and therefore take up a proportionally larger share of the limit.

#### Cost Calculation

Every field in the schema has an integer cost value assigned to it. The cost of a query is the maximum of possible fields selected. Running a query is the best way to find out its true cost.\*

**Search**

* 600 points for any Search, Category Merchandising, or Autocomplete request
* Vector Search–related requests will consume more points per second

**Recommendations**

Request complexity determines how many points it costs, typical costs vary from 100-200 points per request. **Examples** of cost point calculations:&#x20;

* Request minimum costs are 10 points
* 10 points for loading and updating a user session
* 50 points for normal recommendation + 1 points per recommended product
* 100 points for personalized merchandising recommendation + 1 points per recommended product
* 2 points for each active custom segment&#x20;

**GraphQL**

Request complexity determines how many points are consumed.

* Request minimum costs are 10 points
* Recommendation request costs over GraphQL are similar as with recommendation calls (typical costs vary from 100-200 points per request, see recommendations section for more details).



\*Although default costs are in place, Nosto also reserves the right to set manual costs on fields.

## **Rate limit status and cost breakdown visibility** <a href="#rate-limit-status-and-cost-breakdown-visibility" id="rate-limit-status-and-cost-breakdown-visibility"></a>

You can check the status of your rate limits and the cost of the request made using the `X-Nosto-Ratelimit-Status` header in the response. It will contain the request cost, and the status of your rate limit budget in cost points when the request was processed: maximumAvailable, currentlyAvailable and restoration rate.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

If you want to see a more detailed breakdown of a request’s costs, you can add either a request parameter or header with the name `nosto-cost-debug` and the response will then include an additional header `X-Nosto-Ratelimit-Cost-Details`. It will contain a cost breakdown per section as csv.

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

In case you run over your rate limit, the service will then respond with `HTTP 429 Too Many Requests` and a `Retry-After` header. The `Retry-After` header contains the number of seconds to wait until you can make a request again.

#### Avoiding rate limit errors

Designing your usage with best practices in mind is the best way to avoid throttling errors. For example, you can stagger API requests in a queue and do other processing tasks while waiting for the next queued job to run. Consider the following best practices when designing your app:

* Optimize your code to only get the data that your service requires.
* Use caching for data that you use often.
* Regulate the rate of your requests for smoother distribution.
* Include code that catches errors. If you ignore these errors and keep trying to make requests, then your app won't be able to gracefully recover.
* Use metadata about your API usage, included with all API responses, to manage the behavior dynamically.
* Your code should stop making additional API requests until enough time has passed to retry. The recommended backoff time is one second.
