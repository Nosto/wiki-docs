# Nosto GraphQL Implementation Guide

A complete reference for implementing Nosto Search, Category Merchandising, and Recommendations via the GraphQL API — including all required Session API calls and analytics event tracking.

---

## Table of Contents

1. [Overview](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#overview)  
2. [API Endpoints & Authentication](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#api-endpoints--authentication)  
3. [Session Management](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#session-management)  
4. [Search Results Page (SERP)](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#search-results-page-serp)  
   - [Basic Search Query](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#basic-search-query)  
   - [Pagination](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#pagination)  
   - [Sorting](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#sorting)  
   - [Filtering & Facets](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#filtering--facets)  
   - [Search Analytics Events](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#search-analytics-events)  
5. [Autocomplete](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#autocomplete)  
   - [Autocomplete GQL Query](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#autocomplete-gql-query)  
   - [Highlights & Redirects](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#highlights--redirects)  
   - [Autocomplete Analytics Events](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#autocomplete-analytics-events)  
6. [Category Pages](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#category-pages)  
   - [Fetching by Category ID & Path](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#fetching-by-category-id--path)  
   - [Fetching by Category Path Only](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#fetching-by-category-path-only)  
   - [Child Category Handling](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#child-category-handling)  
   - [Custom Filters (Advanced)](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#custom-filters-advanced)  
   - [Category Analytics Events](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#category-analytics-events)  
7. [Recommendations](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#recommendations)  
   - [Session-Based Recommendations](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#session-based-recommendations)  
   - [Sending Cart Data](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#sending-cart-data)  
   - [Recommendations by Page Type](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#recommendations-by-page-type)  
   - [Add to Cart with Attribution](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#add-to-cart-with-attribution)  
8. [Cart Tracking & Order Placement](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#cart-tracking--order-placement)  
   - [Sending Cart via updateSession](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#sending-cart-via-updatesession)  
   - [Placing an Order](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#placing-an-order)  
   - [Order Confirmation Page Recommendations](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#order-confirmation-page-recommendations)  
9. [Content Personalisation](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#content-personalisation)  
   - [How Placements Work](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#how-placements-work)  
   - [Loading Placements & Content](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#loading-placements--content)  
   - [The Response Structure](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#the-response-structure)  
   - [Injecting Content Campaigns](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#injecting-content-campaigns)  
   - [HTML Response Mode](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#html-response-mode)  
   - [Page-Type Methods Reference](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#page-type-methods-reference)  
10. [SPA Considerations](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#spa-considerations)  
11. [API Reference](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#api-reference)

---

## Overview

Nosto exposes two complementary API surfaces for headless and custom frontend implementations:

**Search GraphQL API** (`https://search.nosto.com/v1/graphql`) — Used to query product search results, category page listings, and autocomplete suggestions. This is a **query-only** API (no authentication required beyond your account ID).

**Session / Recommendations GraphQL API** (`https://api.nosto.com/v1/graphql`) — Used to manage user sessions, track page-view events for analytics, and retrieve personalised product recommendations. This API uses **mutations** and requires authentication.

In a typical headless integration, both APIs work together:

- The Search API powers your product listing UI (SERP, category pages, autocomplete).  
- The Session API powers recommendations, analytics event tracking, and session management. All analytics (search impressions, clicks, A/B test attribution) are recorded via the `recordAnalyticsEvent` mutation — no JavaScript library is required.

---

## API Endpoints & Authentication

### Search API

```
POST https://search.nosto.com/v1/graphql
Content-Type: application/json
```

Your `accountId` is passed inside the GraphQL body. For most frontend queries, no authentication header is required. However, a `Bearer` token is needed in two cases:

* **Accessing sensitive data** — fields such as sales stats and sales-based sorting are restricted from public access and require authentication.  
* **Returning all documents** — unauthenticated requests must include a `query`, `categoryId`, or `categoryPath`; omitting all three requires auth.

```
Authorization: Bearer SEARCH_KEY
```

The token is an `API_SEARCH`\-scoped key, available in the Nosto admin under Settings → Authentication Tokens.

**Important:** The Search API uses `Bearer` token auth — this is distinct from the platform API which uses `Basic` auth. Never expose the `API_SEARCH` key on the frontend; use it for server-side requests only.

### Session / Recommendations API

```
POST https://api.nosto.com/v1/graphql
Content-Type: application/json
Authorization: Basic <base64(:NOSTO_API_TOKEN)>
```

The token is your Nosto API token, base64-encoded with an empty username prefix (i.e. `btoa(':' + token)`).

**Token scope:** Session management (`newSession`, `updateSession`, `placeOrder`) and analytics tracking (`recordAnalyticsEvent`, `session` query for segments) all require a token with the **`API_APPS`** scope. Contact Nosto support if you do not have one.

---

## Session Management

Every call to the Session/Recommendations API requires a session identifier. This is a persistent customer ID (`cId`) that Nosto uses to build a profile and personalise recommendations.

### Flow

1. On page load, check for the `2c.cId` cookie.  
2. If the cookie does not exist, call the `newSession` mutation to create one.  
3. Store the returned session ID in a cookie (`2c.cId`, 30-minute expiry).  
4. On every subsequent page view, pass the stored session ID to `updateSession`.

### Creating a New Session

```
mutation {
  newSession(referer: "https://google.com?q=shoes")
}
```

The mutation returns a session ID string. Persist this in a cookie:

```javascript
// Check for existing session
let sessionId = getCookie('2c.cId');

if (!sessionId) {
  const response = await fetch('https://api.nosto.com/v1/graphql', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Basic ' + btoa(':' + NOSTO_API_TOKEN)
    },
    body: JSON.stringify({
      query: `mutation newSession($referer: String) {
        newSession(referer: $referer)
      }`,
      variables: { referer: document.referrer }
    })
  });

  const result = await response.json();
  sessionId = result.data.newSession;

  // Store in cookie (30 min expiry)
  document.cookie = `2c.cId=${sessionId}; Max-Age=1800; Path=/; SameSite=Lax`;
}
```

**Note:** How you persist the session ID is up to your implementation. Cookie storage is the most common approach and aligns with Nosto's own JS library behaviour.

**Shopify note:** Shopify product and variant IDs use global IDs like `gid://shopify/Product/12312312312`. Nosto expects only the numeric portion. Always extract it before sending: `gid://shopify/Product/123` → `"123"`.

---

## Search Results Page (SERP)

The Search API endpoint is `https://search.nosto.com/v1/graphql`. All search queries use the `search` root field.

### Basic Search Query

At minimum, provide `accountId` and `query`. For accurate analytics and A/B test support you should also pass `segments` (retrieved from the Session API before searching) and any stored `abTests` from the current session. Always request `abTests` in the response so you can persist them for subsequent searches.

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green shoes"
    segments: ["5a497a000000000000000001"]  # from session query — see Analytics section
    abTests: []                              # accumulated from prior searches this session
  ) {
    products {
      hits {
        productId
        name
        url
        imageUrl
        price
        listPrice
        brand
        availability
      }
      total
      size
      from
      fuzzy
    }
    abTests {
      id
      activeVariation {
        id
      }
    }
  }
}
```

All indexable product fields are available under `hits`. See the full schema at: [https://search.nosto.com/v1/graphql?ref=SearchProduct](https://search.nosto.com/v1/graphql?ref=SearchProduct)

### Pagination

Use `products.size` to control the number of results per page and `products.from` for the offset.

The default page size is `5`. Change it explicitly in every query.

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "shoes"
    products: { size: 24, from: 0 }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
      from
    }
  }
}
```

For the second page with 24 results per page:

```
products: { size: 24, from: 24 }
```

### Sorting

By default, results are sorted by relevance score. Only apply explicit sorting when a user actively selects a sort option — do not override relevance by default.

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "shoes"
    products: {
      sort: [
        {
          field: "price"
          order: asc
        }
      ]
    }
  ) {
    products {
      hits {
        productId
        name
        price
      }
    }
  }
}
```

`order` accepts `asc` or `desc`. You can sort by any indexed field (e.g. `price`, `name`, `ratingValue`).

### Filtering & Facets

#### Requesting Facets

Include facets in the response so your UI can render filter options:

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "shoes"
    products: { size: 24 }
  ) {
    products {
      hits {
        productId
        name
        price
        brand
      }
      facets {
        ... on SearchTermsFacet {
          id
          field
          type
          name
          data {
            value
            count
            selected
          }
        }
        ... on SearchStatsFacet {
          id
          field
          type
          name
          min
          max
        }
      }
      total
    }
  }
}
```

Facets must first be configured in the Nosto dashboard (Search & Categories → Settings → Facets).

#### Filtering by a Terms Facet

Multiple values for the same field are joined with `OR`. Filters across different fields are joined with `AND`.

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "shoes"
    products: {
      size: 24
      filter: [{ field: "brand", value: ["Adidas", "Converse"] }]
    }
  ) {
    products {
      hits {
        productId
        name
      }
      facets {
        ... on SearchTermsFacet {
          id
          field
          name
          data {
            value
            count
            selected
          }
        }
      }
    }
  }
}
```

#### Filtering by a Stats Facet (e.g. price range)

```
products: {
  filter: [{ field: "price", value: { min: 10, max: 100 } }]
}
```

### Search Analytics Events

Search analytics are tracked via the `recordAnalyticsEvent` mutation on the platform API (`https://api.nosto.com/v1/graphql`). No JavaScript library is needed. The same `API_APPS`\-scoped token used for session management is used here.

The full analytics workflow for a search page is:

1. **Retrieve segments** from the session before searching.  
2. **Execute the search** passing retrieved segments (and any stored A/B variations from the current session).  
3. **Store returned A/B variations** — these must be included in all subsequent search requests in the session.  
4. **Record an impression** immediately after displaying results.  
5. **Record a click** when a user clicks a product.

#### 1\. Retrieve Segments (before searching)

Segments personalise merchandising rules. Fetch them from the session before each search query (they can change during the session as the user browses):

```
query {
  session(by: BY_CID, id: "YOUR_SESSION_ID") {
    segments {
      id
    }
  }
}
```

Pass the returned segment IDs as the `segments` array in your search query. For the first search in a fresh session, `abTests` can be an empty array.

#### 2\. Execute Search with Segments & A/B Tests

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "running shoes"
    segments: ["5a497a000000000000000001", "5b71f1500000000000000006"]
    abTests: []   # empty on first search; accumulate and include from prior searches
  ) {
    products {
      total
      fuzzy
      hits {
        productId
        name
      }
    }
    abTests {
      id
      activeVariation {
        id
      }
    }
  }
}
```

After receiving the response, **store the returned `abTests`** in session storage. All subsequent searches within the 30-minute session must include the accumulated A/B variation assignments — this ensures users see a consistent experience throughout the session. Only include variations that were actually returned by search responses; do not include variations from prior sessions.

#### 3\. Record Search Impression

Send this immediately after rendering search results. The `metadata` object is constructed from the search request and response:

```
mutation {
  recordAnalyticsEvent(
    id: "YOUR_SESSION_ID"
    by: BY_CID
    params: {
      type: SEARCH
      timestamp: "2025-09-02T13:56:08.890Z"
      searchImpression: {
        metadata: {
          hasResults: true          # total > 0
          autoComplete: false       # false for SERP, true for autocomplete
          autoCorrect: false        # set to the `fuzzy` value from search response
          keyword: false            # true if keywords were requested
          organic: true             # true for user-initiated search; false for ad/link referrals
          refined: false            # true if user changed the query mid-session
          refinedQuery: null        # previous query string if refined is true; otherwise null
          sorted: false             # true when sorting by anything other than relevance score
          query: "running shoes"
          resultId: "d65b040c-56ae-4c6d-a038-fe908e140855"  # generate a UUID4 per search
        }
        page: 1                     # 1-based page number
        productIds: ["123", "124", "125"]  # productId values shown on this page
        properties: {
          abTestAttribution: [
            # Include the A/B variations that applied to THIS search request
            { key: "65ca1ee5d05d1f5159f0ac7e", value: "A" }
          ]
        }
      }
    }
  ) {
    errors { message }
    message
  }
}
```

**Metadata field reference:**

| Field | Description |
| :---- | :---- |
| `hasResults` | `true` if the search response `total` is greater than zero |
| `autoComplete` | `false` for SERP; `true` for autocomplete requests |
| `autoCorrect` | Set to the `fuzzy` boolean returned in the search response |
| `keyword` | `true` if keyword suggestions were requested (autocomplete only) |
| `organic` | `true` if the user typed the query; `false` if they arrived via an ad link |
| `refined` | `true` if the user had searched with a different query earlier in the session |
| `refinedQuery` | The previous query if `refined` is `true`; otherwise `null` |
| `sorted` | `true` when a non-default sort is applied (anything other than relevance) |
| `query` | The exact query string entered by the user |
| `resultId` | A UUID4 generated uniquely per search interaction — store this for click tracking |

#### 4\. Record Search Click

Send this when a user clicks a product in the search results. Reuse the same `metadata` and `abTestAttribution` from the impression:

```
mutation {
  recordAnalyticsEvent(
    id: "YOUR_SESSION_ID"
    by: BY_CID
    params: {
      type: SEARCH
      timestamp: "2025-09-02T13:57:12.000Z"
      searchClick: {
        metadata: {
          hasResults: true
          autoComplete: false
          autoCorrect: false
          keyword: false
          organic: true
          refined: false
          refinedQuery: null
          sorted: false
          query: "running shoes"
          resultId: "d65b040c-56ae-4c6d-a038-fe908e140855"  # same resultId as the impression
        }
        productId: "123"            # ID of the product that was clicked
        properties: {
          abTestAttribution: [
            { key: "65ca1ee5d05d1f5159f0ac7e", value: "A" }
          ]
        }
      }
    }
  ) {
    errors { message }
    message
  }
}
```

#### 5\. Filters, Sorting & Pagination

When a user changes a filter, sort order, or navigates to a new results page, treat it as a new search interaction: re-execute the search query (with the updated parameters) and record a new impression. The `refined` flag should only be `true` when the user changes the query text itself — not for filter or sort changes. Update `sorted: true` whenever a non-default sort is applied.

---

## Autocomplete

Autocomplete queries use the same `search` root field as SERP and can return up to four suggestion types alongside products: `keywords`, `categories`, and `popularSearches`. Trigger autocomplete after the user has typed at least 2 characters.

### Autocomplete GQL Query

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: { size: 5 }
    keywords: { size: 5 }
    categories: { size: 5 }
    popularSearches: {
      size: 5
      emptyQueryMatchesAll: true   # return default suggestions when query is empty
    }
  ) {
    products {
      hits {
        productId
        name
        url
        imageUrl
        price
        listPrice
      }
      total
    }
    keywords {
      hits {
        keyword
        _redirect
        _highlight {
          keyword
        }
      }
    }
    categories {
      hits {
        name
        fullName
        externalId
        parentExternalId
        url
        urlPath
      }
      total
    }
    popularSearches {
      hits {
        query
        total
      }
      total
    }
    query
  }
}

```

**Example response:**

```json
{
  "data": {
    "search": {
      "query": "green",
      "products": {
        "hits": [
          { "productId": "1", "name": "Green Running Shoe" }
        ],
        "total": 14
      },
      "keywords": {
        "hits": [
          {
            "keyword": "green energy",
            "_redirect": null,
            "_highlight": { "keyword": "<em>green</em> energy" }
          }
        ]
      },
      "categories": {
        "hits": [
          {
            "name": "Fashion > Jackets > Green Jackets",
            "fullName": "Fashion > Jackets > Green Jackets",
            "externalId": "4321",
            "parentExternalId": "8765",
            "url": "https://www.example.com/category/fashion",
            "urlPath": "fashion"
          }
        ],
        "total": 86
      },
      "popularSearches": {
        "hits": [
          { "query": "green pants", "total": 3024 },
          { "query": "green shirt", "total": 480 }
        ],
        "total": 2
      }
    }
  }
}

```

###  **Empty Query (Default Suggestions)**

To show suggestions when the search field is empty or has no typed characters, set `emptyQueryMatchesAll: true`. This applies to all suggestion types — keywords, categories, and popular searches — and is useful for displaying trending/default suggestions on focus before the user starts typing:

```json
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: ""
    popularSearches: { size: 5, emptyQueryMatchesAll: true }
    keywords: { size: 5, emptyQueryMatchesAll: true }
    categories: { size: 5, emptyQueryMatchesAll: true }
  ) {
    popularSearches {
      hits {
        query
        total
      }
    }
    keywords {
      hits {
        keyword
      }
    }
    categories {
      hits {
        name
        url
      }
    }
    query
  }
}

```

### **Highlights & Redirects**

* **Highlights** (`_highlight.keyword`): Pre-formatted HTML with `<em>` tags wrapping the matched portion. Render this directly to emphasise matching text.  
* **Redirects** (`_redirect`): If a keyword has a configured redirect URL, navigate the user there instead of to the SERP when they click that keyword. The redirect itself must be implemented in your frontend — Nosto only returns the URL.

Keywords must appear in at least 3 different products to qualify as an autocomplete suggestion. You can add custom keywords in the Nosto dashboard to bypass this threshold.

**Best practices for autocomplete UI:**

* Show up to 5 product suggestions with image, name, and price.  
* Bold matched keyword portions (use `_highlight.keyword` which returns pre-formatted `<em>` tags).  
* Link category suggestions directly to their `url` field — clicking a category result should navigate to that category page, not the SERP.  
* Show popular searches as clickable query terms that submit a full SERP search when clicked.  
* Include a "See all results" CTA that takes the user to the SERP.  
* Do not include facets in the autocomplete dropdown.

### Autocomplete Analytics Events

Autocomplete tracking uses the same `recordAnalyticsEvent` mutation as SERP, with one key difference: set `autoComplete: true` in the metadata. Use `type: SEARCH` for all autocomplete events.

#### Impression (each time dropdown is shown)

Record an impression each time autocomplete results are rendered (typically debounced to avoid excessive calls while the user is typing):

```
mutation {
  recordAnalyticsEvent(
    id: "YOUR_SESSION_ID"
    by: BY_CID
    params: {
      type: SEARCH
      timestamp: "2025-09-02T13:56:08.890Z"
      searchImpression: {
        metadata: {
          hasResults: true
          autoComplete: true          # always true for autocomplete
          autoCorrect: false
          keyword: false              # set true if keyword suggestions were included
          organic: true
          refined: false
          refinedQuery: null
          sorted: false
          query: "shoe"
          resultId: "a1b2c3d4-..."   # new UUID4 per autocomplete interaction
        }
        page: 1
        productIds: ["123", "124"]
        properties: {
          abTestAttribution: []
        }
      }
    }
  ) {
    errors { message }
  }
}
```

#### Product Clicked from Autocomplete

```
mutation {
  recordAnalyticsEvent(
    id: "YOUR_SESSION_ID"
    by: BY_CID
    params: {
      type: SEARCH
      timestamp: "2025-09-02T13:56:15.000Z"
      searchClick: {
        metadata: {
          hasResults: true
          autoComplete: true
          autoCorrect: false
          keyword: false
          organic: true
          refined: false
          refinedQuery: null
          sorted: false
          query: "shoe"
          resultId: "a1b2c3d4-..."   # same resultId as the autocomplete impression
        }
        productId: "123"
        properties: {
          abTestAttribution: []
        }
      }
    }
  ) {
    errors { message }
  }
}
```

#### Keyword Suggestion Clicked

When a keyword suggestion is clicked (not a product), set `keyword: true` in the metadata. The user is then taken to the SERP — record the subsequent SERP impression with `keyword: true` as well to indicate it was initiated via a keyword suggestion.

---

## Category Pages

Category pages use the same `search` endpoint but instead of a `query` term, you provide a `categoryId` and/or `categoryPath` to fetch products for that category.

### Fetching by Category ID & Path

Using `categoryId` is only **fully supported for Shopify merchants**. For all other platforms, use `categoryPath` alone (or both fields for richer analytics data).

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryId: "123456789"
      categoryPath: "Pants"
      size: 24
      from: 0
    }
  ) {
    products {
      hits {
        productId
        name
        url
        imageUrl
        price
        listPrice
        brand
        availability
      }
      total
      size
      from
      facets {
        ... on SearchTermsFacet {
          id
          field
          name
          data { value count selected }
        }
        ... on SearchStatsFacet {
          id
          field
          name
          min
          max
        }
      }
    }
  }
}
```

### Fetching by Category Path Only

`categoryPath` maps to the `categories` field on a product and is the standard approach for non-Shopify platforms.

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryPath: "Pants"
      size: 24
      from: 0
    }
  ) {
    products {
      hits {
        productId
        name
        url
        imageUrl
        price
      }
      total
      size
    }
  }
}
```

For nested categories, use the fully qualified category path:

```
categoryPath: "Women/Dresses/Evening Dresses"
```

### Child Category Handling

By default, fetching a parent category may also return products from its child categories (e.g. querying `"Pants"` also returns products from `"Pants/Shorts"` and `"Pants/Khakis"`). This behaviour is controlled by an admin setting — contact your Nosto representative to adjust it.

### Custom Filters (Advanced)

In rare cases where `categoryId` or `categoryPath` is not sufficient (e.g. custom landing pages), use `preFilter` to build arbitrary product queries:

```
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      preFilter: [
        {
          field: "productId"
          value: ["2276", "2274", "2280"]
        }
      ]
      size: 24
    }
  ) {
    products {
      hits {
        productId
        name
      }
      total
    }
  }
}
```

### Category Analytics Events

Category analytics use the same `recordAnalyticsEvent` mutation, but with `type: CATEGORY` and a simpler metadata structure. The workflow mirrors search: retrieve segments before querying, include segments and stored A/B variations in the category query, then record an impression and (when applicable) a click.

#### 1\. Category Impression

Record immediately after displaying category results:

```
mutation {
  recordAnalyticsEvent(
    id: "YOUR_SESSION_ID"
    by: BY_CID
    params: {
      type: CATEGORY
      timestamp: "2025-09-02T13:56:08.890Z"
      categoryImpression: {
        metadata: {
          category: "Pants"         # human-readable; same as categoryPath in the search request
          categoryId: "123456"      # machine-readable; same as categoryId in the search request
        }
        page: 1
        productIds: ["123", "124", "125"]
        properties: {
          abTestAttribution: [
            { key: "65ca1ee5d05d1f5159f0ac7e", value: "A" }
          ]
        }
      }
    }
  ) {
    errors { message }
    message
  }
}
```

At least one of `category` or `categoryId` is required. Provide the same value(s) you passed to the search query. If your platform only has a path (no ID), you can omit `categoryId`.

#### 2\. Product Clicked from Category Page

```
mutation {
  recordAnalyticsEvent(
    id: "YOUR_SESSION_ID"
    by: BY_CID
    params: {
      type: CATEGORY
      timestamp: "2025-09-02T13:57:05.000Z"
      categoryClick: {
        metadata: {
          category: "Pants"
          categoryId: "123456"
        }
        productId: "123"
        properties: {
          abTestAttribution: [
            { key: "65ca1ee5d05d1f5159f0ac7e", value: "A" }
          ]
        }
      }
    }
  ) {
    errors { message }
    message
  }
}
```

#### 3\. Filters, Sorting & Pagination

Re-execute the category search query with updated parameters and record a new `categoryImpression`. The metadata (`category`, `categoryId`) stays the same — only `page` and `productIds` change. No separate attribution call is needed for filter/sort/pagination interactions.

---

## Recommendations

Recommendations are fetched via the Session API using `updateSession` mutations. The mutation both records the page-view event for analytics and returns personalised product recommendations for the given page type.

### Session-Based Recommendations

All recommendation requests follow this pattern:

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: <EVENT_TYPE>
        target: "<TARGET>"
      }
    }
  ) {
    pages {
      for<PageType>(params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
      }) {
        divId
        resultId
        resultTitle
        primary {
          productId
          name
          url
          imageUrl
          price
          currencyCode
          availability
        }
      }
    }
  }
}
```

### Sending Cart Data

When mutating a session, always include the current cart contents. This ensures Nosto's personalisation engine has accurate data. Omit `cart` only if the cart is empty.

```
mutation {
  updateSession(by: BY_CID, id: "ad8f0d0e-1156-4df2-b385-10e03f8f8a44",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "400"
      }
      cart: {
        items: [
          {
            productId: "100"
            skuId: "100-1"
            name: "Blue Runner"
            unitPrice: 199
            priceCurrencyCode: "EUR"
            quantity: 1
          },
          {
            productId: "200"
            skuId: "200-1"
            name: "Red Trainer"
            unitPrice: 299
            priceCurrencyCode: "EUR"
            quantity: 2
          }
        ]
      }
    }
  ) {
    id
  }
}
```

### Recommendations by Page Type

#### Search Page

Event type `SEARCHED_FOR`. Pass the search term as `target`.

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: SEARCHED_FOR
        target: "black shoes"
      }
    }
  ) {
    pages {
      forSearchPage(params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
      }, term: "black shoes") {
        divId
        resultId
        primary {
          productId
          name
          url
          imageUrl
          price
        }
      }
    }
  }
}
```

#### Category Page

Event type `VIEWED_CATEGORY`. Use the fully qualified category path (FQCP) as `target` — e.g. for "Dresses" under "Women", use `"/Women/Dresses"`.

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: VIEWED_CATEGORY
        target: "/Women/Dresses"
      }
    }
  ) {
    pages {
      forCategoryPage(params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
      }, category: "Women/Dresses") {
        divId
        resultId
        primary {
          productId
          name
          url
          imageUrl
          price
        }
      }
    }
  }
}
```

To fetch only a specific recommendation slot, use the `slotIds` parameter:

```
forCategoryPage(params: {
  isPreview: false
  imageVersion: VERSION_8_400_400
  slotIds: ["categorypage-nosto-1"]
}, category: "Women/Dresses") {
  divId
  resultId
  primary { productId }
}
```

#### Product Page

Event type `VIEWED_PRODUCT`. Pass the product ID as `target`. If the customer was referred from a recommendation slot, include the slot's `resultId` as `ref` for accurate attribution.

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "11923861519"
        ref: "front-page-slot-1"
      }
    }
  ) {
    pages {
      forProductPage(params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
        slotIds: ["pdp-rec-1", "pdp-rec-2"]
      }, product: "11923861519") {
        divId
        resultId
        resultTitle
        primary {
          productId
          name
          url
          imageUrl
          price
          currencyCode
          availability
        }
      }
    }
  }
}
```

#### Front Page

Event type `VIEWED_PAGE`. Pass the homepage URL as `target`.

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: VIEWED_PAGE
        target: "https://example.com"
      }
    }
  ) {
    pages {
      forFrontPage(params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
      }) {
        divId
        resultId
        primary {
          productId
          name
          url
          imageUrl
          price
        }
      }
    }
  }
}
```

#### Cart / Checkout Page

Event type `VIEWED_CART`.

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: VIEWED_CART
        target: "https://example.com/cart"
      }
    }
  ) {
    pages {
      forCartPage(params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
      }) {
        divId
        resultId
        primary {
          productId
          name
          url
          imageUrl
          price
        }
      }
    }
  }
}
```

### Add to Cart with Attribution

If a customer adds a product to the cart **without a page navigation** (e.g. via a recommendation widget's add-to-cart button), use `skipEvents: true` to prevent logging a spurious page-view event. Pass the recommendation slot's `resultId` as `ref` for correct revenue attribution.

```
mutation MySession {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      skipEvents: true
      event: {
        type: VIEWED_PAGE
        target: "9150249402681"
        ref: "frontpage-bestseller"
      }
      cart: {
        items: [
          {
            productId: "100"
            skuId: "100-1"
            name: "Product Name"
            unitPrice: 199
            priceCurrencyCode: "EUR"
            quantity: 1
          }
        ]
      }
    }
  ) {
    id
  }
}
```

`target` is the product ID being added. `ref` is the `divId` or `resultId` of the recommendation slot that was clicked.

### Sending Customer Data

When a customer is logged in, send their details alongside the session mutation. This merges the customer's online behaviour profile. Omit this block if no customer is logged in.

```
mutation MySession {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      customer: {
        firstName: "John"
        lastName: "Doe"
        marketingPermission: true
        customerReference: "319330"
      }
      event: {
        type: VIEWED_PRODUCT
        target: "400"
      }
    }
  ) {
    id
  }
}
```

---

## Cart Tracking & Order Placement

Nosto needs an accurate, up-to-date view of a customer's cart at all times. This powers abandoned cart emails, Facebook pixel retargeting events, and recommendation personalisation. In a GQL implementation, cart data is sent as part of every `updateSession` mutation — no separate JS API call is needed. Orders are submitted via the `placeOrder` GQL mutation.

**Important:** The `productId` used in cart items, order line items, and product events must all match exactly. Mismatches will break attribution and statistics.

### Sending Cart via updateSession

Include the `cart` block in every `updateSession` call, on every page where the cart may have changed. Always send the **complete, current cart contents** — not just the item that changed. If the cart is empty, omit the `cart` block entirely.

```
mutation {
  updateSession(by: BY_CID, id: "<SESSION_ID>",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "181503"
      }
      cart: {
        items: [
          {
            productId: "181503"
            skuId: "181505"
            name: "Men's Running Shirt"
            unitPrice: 123.45
            priceCurrencyCode: "EUR"
            quantity: 2
          },
          {
            productId: "34552"
            skuId: "39912"
            name: "Men's Training Shoe"
            unitPrice: 999.00
            priceCurrencyCode: "EUR"
            quantity: 1
          }
        ]
      }
    }
  ) {
    id
  }
}
```

The cart block is the same regardless of which page type you're on — simply include it alongside whatever event and `pages` fields you're already sending for that page. For example, a category page call with cart data looks exactly like the category recommendation mutation shown in the [Recommendations](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#recommendations) section, with the `cart` block added.

| Cart field | Type | Description |
| :---- | :---- | :---- |
| `productId` | String | Parent product ID — must match catalogue |
| `skuId` | String | Variant/SKU ID |
| `name` | String | Product name |
| `unitPrice` | Float | Price per unit |
| `priceCurrencyCode` | String | ISO 4217 currency code e.g. `"EUR"` |
| `quantity` | Int | Quantity in cart |

### Placing an Order

Order data must be submitted to Nosto on the order confirmation page. This uses the `placeOrder` GQL mutation on the Session/Recommendations API (`https://api.nosto.com/v1/graphql`).

Orders can be associated with a customer either by the Nosto session cookie ID (`BY_CID`) or by your own customer reference (`BY_REF`).

**By session cookie ID (most common):**

```
mutation {
  placeOrder(by: BY_CID, id: "<SESSION_ID>", params: {
    customer: {
      firstName: "Jane"
      lastName: "Smith"
      email: "jane@example.com"
      marketingPermission: true
    }
    order: {
      number: "ORD-25435"
      orderStatus: "paid"
      paymentProvider: "stripe"
      ref: "0010"
      purchasedItems: [
        {
          name: "Men's Running Shirt"
          productId: "181503"
          skuId: "181505"
          priceCurrencyCode: "EUR"
          unitPrice: 123.45
          quantity: 2
        },
        {
          name: "Men's Training Shoe"
          productId: "34552"
          skuId: "39912"
          priceCurrencyCode: "EUR"
          unitPrice: 999.00
          quantity: 1
        }
      ]
    }
  }) {
    id
  }
}
```

**By customer reference (for known/logged-in customers):**

```
mutation {
  placeOrder(by: BY_REF, id: "customer-reference-123", params: {
    customer: {
      firstName: "Jane"
      lastName: "Smith"
      email: "jane@example.com"
      marketingPermission: true
    }
    order: {
      number: "ORD-25435"
      orderStatus: "paid"
      paymentProvider: "stripe"
      ref: "0010"
      purchasedItems: [
        {
          name: "Men's Running Shirt"
          productId: "181503"
          skuId: "181505"
          priceCurrencyCode: "EUR"
          unitPrice: 123.45
          quantity: 1
        }
      ]
    }
  }) {
    id
  }
}
```

| Field | Description |
| :---- | :---- |
| `number` | Your order number — must be unique per order |
| `orderStatus` | e.g. `"paid"`, `"pending"`, `"cancelled"` |
| `paymentProvider` | e.g. `"stripe"`, `"klarna"`, `"paypal"` |
| `ref` | The `result_id` of the recommendation slot that was last clicked before purchase (for attribution) |
| `productId` | Parent product ID — must match catalogue and cart tagging |
| `skuId` | Variant/SKU ID |

### Order Confirmation Page Recommendations

You can fetch recommendations for the order confirmation page in the same `placeOrder` mutation:

```
mutation {
  placeOrder(by: BY_CID, id: "<SESSION_ID>", params: {
    customer: {
      firstName: "Jane"
      lastName: "Smith"
      email: "jane@example.com"
      marketingPermission: false
    }
    order: {
      number: "ORD-25435"
      orderStatus: "paid"
      paymentProvider: "stripe"
      ref: "0010"
      purchasedItems: [
        {
          name: "Men's Running Shirt"
          productId: "181503"
          skuId: "181505"
          priceCurrencyCode: "EUR"
          unitPrice: 123.45
          quantity: 1
        }
      ]
    }
  }) {
    id
    pages {
      forOrderPage(value: "ORD-25435", params: {
        isPreview: false
        imageVersion: VERSION_8_400_400
      }) {
        divId
        resultId
        resultTitle
        primary {
          productId
          name
          url
          imageUrl
          price
        }
      }
    }
  }
}
```

---

## Content Personalisation

**Important:** Content Personalisation is **not available via GraphQL**. It must be implemented using the `nostojs` JavaScript Session API. This applies to all Onsite Content Personalisation (OCP) campaigns — including banners, hero images, pop-ups, and any HTML-based personalised content served into placement divs.

### How Placements Work

Nosto content is served into **placements** — `<div>` elements with a unique `id` attribute and `nosto_element` class that you embed in your page templates. Nosto scans the page for these divs and injects personalised content into the matching ones based on the current user's segment, the page type, and any configured targeting rules.

```html
<!-- Example static placement div in your template -->
<div class="nosto_element" id="frontpage-banner"></div>
<div class="nosto_element" id="frontpage-center-1"></div>
```

There are two placement types:

- **Static placements** — hardcoded `<div>` elements in your page templates (the standard approach).  
- **Dynamic placements** — Nosto injects elements by targeting existing DOM elements via CSS selectors, configured in the Nosto admin.

### Loading Placements & Content

Use `api.defaultSession()` with the appropriate page-type method, then call `.setPlacements()` and `.load()`. Placements can be scanned automatically from the DOM with `api.placements.getPlacements()`, or passed manually as an array.

**Automatic placement scan (recommended):**

```javascript
nostojs(api => {
  api.defaultSession()
    .viewFrontPage()
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => {
      console.log(response.campaigns)
    })
})
```

**Manual placement list:**

```javascript
nostojs(api => {
  api.defaultSession()
    .viewFrontPage()
    .setPlacements(['frontpage-banner', 'frontpage-center-1'])
    .load()
    .then(response => {
      console.log(response.campaigns)
    })
})
```

### The Response Structure

The `response.campaigns` object contains two top-level keys: `content` (OCP HTML campaigns) and `recommendations` (product recommendation slots). Each is keyed by placement ID.

```json
{
  "campaigns": {
    "content": {
      "frontpage-banner": {
        "div_id": "frontpage-banner",
        "result_id": "5fc6390c60b2ecd3cc0c2d4f",
        "html": "<div class='nosto-banner'>...</div>",
        "params": {}
      }
    },
    "recommendations": {
      "frontpage-center-1": {
        "div_id": "frontpage-center-1",
        "result_id": "frontpage-center-1",
        "title": "Most Popular Right Now",
        "products": [
          { "product_id": "123", "url": "...", "name": "..." }
        ],
        "params": {}
      }
    }
  }
}
```

### Injecting Content Campaigns

Content campaigns return pre-rendered HTML. Always inject them via the Nosto API helper `api.placements.injectCampaigns()` rather than manually — this ensures correct attribution and executes any embedded JavaScript in the campaign HTML.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewFrontPage()
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => {
      // Inject HTML content campaigns into their placement divs
      api.placements.injectCampaigns(response.campaigns.content)

      // Handle product recommendation campaigns yourself
      const recs = response.campaigns.recommendations
      Object.keys(recs).forEach(placementId => {
        const slot = recs[placementId]
        renderProductRecommendations(placementId, slot.products, slot.result_id)
      })
    })
})
```

**Never inject content campaign HTML manually** (e.g. via `innerHTML`). Always use `api.placements.injectCampaigns()` — it handles placement targeting, attribution tracking, and JavaScript execution within the campaign HTML.

### HTML Response Mode

By default, recommendation campaigns return raw JSON product data which you render yourself. If you want Nosto to return pre-rendered HTML for recommendations as well (using your configured recommendation templates), set the response mode to `'HTML'`:

```javascript
nostojs(api => {
  api.defaultSession()
    .setResponseMode('HTML')
    .viewProduct('product-123')
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => {
      // In HTML mode, recommendations also have an .html field
      // Still use injectCampaigns for both content and recommendations
      api.placements.injectCampaigns(response.campaigns.content)
      api.placements.injectCampaigns(response.campaigns.recommendations)
    })
})
```

Note: even in HTML mode, Nosto does **not** automatically inject the HTML into the DOM. Your application must call `injectCampaigns()` to do so.

### Page-Type Methods Reference

Use the correct page-type method to set context for both content targeting and recommendations. Call `.load()` once per page view; use `.update()` for subsequent calls on the same page (e.g. after an add-to-cart) to avoid inflating page-view counts.

| Page | Method | Notes |
| :---- | :---- | :---- |
| Home page | `.viewFrontPage()` | — |
| Product page | `.viewProduct('product-id')` | Tracks a product view event |
| Category page | `.viewCategory('/category-path')` | Use fully qualified path e.g. `'/Women/Dresses'` |
| Search results | `.viewSearch('search term')` | — |
| Cart page | `.viewCart()` | — |
| Order confirmation | Use `placeOrder` GQL mutation | See [Order Confirmation Page Recommendations](https://docs.google.com/document/d/1Q7uVjy4fvSG5pdvoa-HGbrIzWYroa3L3WYRg7ZFeBWY/edit#order-confirmation-page-recommendations) |
| 404 / Not found | `.viewNotFound()` | — |
| Other / landing | `.viewOther()` | Generic fallback |

**Full example — product page with content and recommendations:**

```javascript
nostojs(api => {
  api.defaultSession()
    .viewProduct('181503')
    .setRef('181503', 'frontpage-nosto-1') // attribution: product clicked from this slot
    .setCart({
      items: [
        {
          product_id: "99001",
          sku_id: "99001-L",
          name: "Existing Cart Item",
          unit_price: 45.00,
          price_currency_code: "EUR",
          quantity: 1
        }
      ]
    })
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => {
      // Inject content personalisation campaigns (banners, etc.)
      api.placements.injectCampaigns(response.campaigns.content)

      // Render product recommendation slots
      const recs = response.campaigns.recommendations
      Object.keys(recs).forEach(placementId => {
        renderProductRecommendations(
          placementId,
          recs[placementId].products,
          recs[placementId].result_id
        )
      })
    })
})
```

**Reporting an add-to-cart from a recommendation slot** (without page navigation):

```javascript
nostojs(api => {
  api.defaultSession()
    .reportAddToCart('product-id', 'nosto-productpage-1') // product id + slot result_id
    .update()
})
```

`.reportAddToCart()` only tells Nosto about the attribution. You must still call `.setCart()` with the updated cart contents separately.

---

In Single Page Application (SPA) architectures, there are no full page reloads. Since all analytics tracking is done via direct GraphQL mutations to `https://api.nosto.com/v1/graphql`, there is no buffering or flush step required — each `recordAnalyticsEvent` call is an independent HTTP request that completes immediately.

**SPA-specific considerations for analytics:**

- Fire `recordAnalyticsEvent` impression calls as soon as results are rendered, regardless of navigation type.  
- Store the `resultId` (UUID) you generated for the impression in component state so it is available for click tracking when the user interacts.  
- Reset your stored A/B variation list and session ID when the 30-minute session expires (i.e. when `store.sessionStart` is older than 30 minutes).  
- On route changes, always re-fetch segments before executing the next search query, as segment assignments can change based on browsing behaviour.

---

## API Reference

| Resource | URL |
| :---- | :---- |
| Search API Playground | [https://search.nosto.com/v1/graphql](https://search.nosto.com/v1/graphql) |
| SearchProduct schema | [https://search.nosto.com/v1/graphql?ref=SearchProduct](https://search.nosto.com/v1/graphql?ref=SearchProduct) |
| InputSearchQuery schema | [https://search.nosto.com/v1/graphql?ref=InputSearchQuery](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) |
| InputSearchProducts schema | [https://search.nosto.com/v1/graphql?ref=InputSearchProducts](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) |
| InputSearchFilter schema | [https://search.nosto.com/v1/graphql?ref=InputSearchFilter](https://search.nosto.com/v1/graphql?ref=InputSearchFilter) |
| Nosto Tech Docs | [https://docs.nosto.com/techdocs](https://docs.nosto.com/techdocs) |
| Analytics & A/B Testing (official guide) | [https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-api/analytics-ab-testing](https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-api/analytics-ab-testing) |
| GraphQL for Headless | [https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-for-headless](https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-for-headless) |
| GQL Onsite Sessions | [https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions](https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions) |
| GQL Placing Orders | [https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-mutations/working-with-orders/graphql-placing-orders](https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-mutations/working-with-orders/graphql-placing-orders) |
| Session API: Managing Sessions (cart) | [https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-managing-sessions](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-managing-sessions) |
| Session API: Leveraging Features (content) | [https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features) |
| Session API: Handling Placements | [https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/handling-placements](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/handling-placements) |
| nosto-js open source library | [https://github.com/Nosto/nosto-js](https://github.com/Nosto/nosto-js) |

