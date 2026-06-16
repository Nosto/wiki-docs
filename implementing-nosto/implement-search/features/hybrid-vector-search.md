---
description: >-
    Merchants with Hybrid Vector Search enabled gain the ability to search products based on conceptual similarity
    in addition to precise retrieval based on textual relevance.
---

# Hybrid Vector Search

Depending on merchant preferences, Hybrid Vector Search can take over from keyword search when:

* The search query has a poor click-through rate.
* The search query has a poor conversion rate.
* The search query has no results.
* Hybrid Vector Search was requested specifically for a particular search query.

In principle, Hybrid Vector Search works out of the box and doesn't require changes to the integration.
The suggestions below are optional.

## Recognizing the type of search logic being used

To understand results, it's valuable to know which type of logic generated them.
The search API exposes this information in the [`SearchProducts` object](https://search.nosto.com/v1/graphql?ref=SearchProducts)
of the search response.

`searchType` contains the type of search logic being used:
* `keyword` indicates normal keyword search.
* `vector` indicates Hybrid Vector Search.

This information can be optionally used in the search result page to convey whether these results are precise
(keyword search) or conceptually related (Hybrid Vector Search).
Hybrid Vector Search results can be more general than keyword search results —
communicating with the user helps with setting expectations.

### Tracking requirements

In order to benefit from analytics for Hybrid Vector Search, the search type must be included in impression- and click tracking events sent to Nosto.

The `searchType` value queried from the search API can be included in tracking events verbatim within the tracking metadata property `searchType`, on the same level as `query`.
Here is an example for a well-formed tracking metadata that includes the search type:

```json
{
  "hasResults": true,
  "autoComplete": false,
  "autoCorrect": false,
  "keyword": false,
  "organic": true,
  "refined": false,
  "refinedQuery": null,
  "sorted": false,
  "query":  "t-shirt",
  "resultId": "d65b040c-56ae-4c6d-a038-fe908e140855",
  "searchType": "vector" // or "keyword"
}
```

When using NostoJS' automatic tracking tracking (using the `track` parameter), `searchType` tracking is included automatically, and no adjustments to the integration are required.

API integrations and integrations managing tracking metadata manually need to take care of passing through `searchType` explicitly.

## Limitations

When Hybrid Vector Search engages, features of Nosto search work as normal with one major caveat:
Only the **1000 most relevant results** are accessible via pagination and covered by facets and sorting.
Depending on the strictness of the relevance threshold defined in the Hybrid Vector Search settings, fewer results
could be available.

This cutoff is based purely on Hybrid Vector Search relevance and does not take pinning or promote/demote rules into account.
