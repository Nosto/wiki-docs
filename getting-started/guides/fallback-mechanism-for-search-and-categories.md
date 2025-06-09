---
description: >-
  Guarantee that Product-Listing Pages (PLP) and Search-Result Pages (SERP)
  always return products—even if a Nosto request fails or returns no data.
---

# Fallback mechanism for Search and Categories

### 1 How Nosto Search/Category Merchandising Works

| Phase               | Action                                                                                                                                                                                        |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Intercept**       | Plugin overrides Shopware’s default routes (`ProductListingRoute`, `ProductSearchRoute`) and sends a request to Nosto.                                                                        |
| **Check response**  | If Nosto returns products, those items render in the storefront.                                                                                                                              |
| **Empty / Timeout** | If Nosto times out, errors, or returns an empty product set, the plugin **automatically falls back** to Shopware’s native route so customers still see default results if they are available. |

***

***

### 2 Timeout Strategy

| Component                                 | Default | Plugin Override                                      |
| ----------------------------------------- | ------- | ---------------------------------------------------- |
| **Nosto PHP SDK** (`SearchRequest`)       | 10 s    | Can be changed at runtime via `setResponseTimeout()` |
| **Nosto Plugin** (`SearchRequestHandler`) | —       | Sets **3s** timeout for every PLP/SERP call          |

_If Nosto does not respond within 3 seconds, the result is treated as empty → fallback is triggered._

***

### 3 Operational Summary

| Situation                        | Storefront Outcome                                     |
| -------------------------------- | ------------------------------------------------------ |
| **Nosto responds with products** | Nosto results displayed.                               |
| **Nosto returns empty set**      | Shopware default results displayed.                    |
| **Nosto request exceeds 3 s**    | Treated as empty → Shopware default results displayed. |
| **Nosto server error**           | Treated as empty → Shopware default results displayed. |

This logic applies uniformly to **Category Listing** and **Search Result** pages, ensuring uninterrupted shopping even when external services are slow or unavailable.

***
