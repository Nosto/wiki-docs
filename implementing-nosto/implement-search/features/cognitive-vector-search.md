---
description: >-
    Cognitive Vector Search finds relevant products based on conceptual similarity to the search terms in situations
    in which keyword search performs poorly or delivers no relevant results.
---

# Cognitive Vector Search

Merchants with Cognitive Vector Search enabled gain the ability to search products based on conceptual similarity
in addition to precise retrieval based on textual relevance.

Depending on merchant preferences, Cognitive Vector Search can take over from keyword search when:

* The search query has a poor click-through rate.
* The search query has a poor conversion rate.
* The search query has no results.
* Cognitive Vector Search was requested specifically for a particular search query.

## Recognizing the type of search logic being used

To understand results, it's valuable to know which type of logic generated them.
The search API exposes this information in the [`SearchProducts` object](https://search.nosto.com/v1/graphql?ref=SearchProducts)
of the search response.

`searchType` contains the type of search logic being used:
* `keyword` indicates normal keyword search.
* `vector` indicates Cognitive Vector Search.

This information can be optionally used in the search result page to convey whether these results are precise
(keyword search) or conceptually related (Cognitive Vector Search).
Cognitive Vector Search results can be more general than keyword search results -
communicating with the user helps with setting expectations.

## Limitations

When Cognitive Vector Search engages, features of Nosto search work as normal with one major caveat:
Only the **1000 most relevant results** are accessible via pagination and covered by facets and sorting.
Depending on the strictness of the relevance threshold defined in the Cognitive Vector Search settings, fewer results
could be available.

This cutoff is based purely on Cognitive Vector Search relevance and does not take pinning or promote/demote rules into account.
