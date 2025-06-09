---
description: Feature flag introduced in Nosto Plugin 3.5.5 / 5.1.5 / 6.0.0
---

# Caching

**Default Shopware Caching**\
Only the **first** load of a Category (PLP) or Search (SERP) page was served by Nosto; subsequent visits were served from Shopware’s core cache, potentially showing stale data. This could affect personalisation and analytics on Nosto,

### 1 Where to find it

```
Shopware Admin → Extensions → My Extensions → Nosto → Configuration
```

***

<figure><img src="../../.gitbook/assets/Screenshot 2025-06-09 at 19.17.04.png" alt=""><figcaption></figcaption></figure>

### 2 Behaviour Overview

| Flag         | Cache interval                   | Result                                                                                                                                                                                        |
| ------------ | -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Disabled** | n/a                              | Caching is disabled for all PLP & SERP requests. Every page load fetches fresh results from Nosto.                                                                                            |
| **Enabled**  | _N_ minutes (set below the flag) | Caching is enabled for PLP/SERP response in its HTTP cache. The first request after _N_ minutes triggers a fresh call to Nosto; all other requests within the interval are served from cache. |

***

### 3 Configuration Steps

1. **Open Nosto plugin settings** (see path above).
2. Toggle **Enable Cache**:
   * Off – real-time results (no cache).
   * On – enable caching.
3. If enabled, set **Cache interval (minutes)** to your preferred TTL, e.g. **15**.\
   &#xNAN;_&#x4D;inimum_: 1 min · _Typical_: 10–30 min · _Maximum_: any value
4. **Save** the configuration. Changes take effect immediately—no Shopware cache clear is required.

***

#### How it works under the hood

* **Flag off** → The plugin tells Shopware to add the route `/product-listing/*` and `/search/*` to the cache exclusion list.
* **Flag on** → The plugin attaches the configured TTL so Shopware’s HTTP cache invalidates pages automatically.

***

### 4 FAQ

**Does this affect CMS/PDP pages?**\
No. Only Category and Search result pages rendered via Nosto are affected.

**What would happen if I enable caching?**\
Category and Search result pages would be caching for a selected timeframe. This would affect Search and CM analytics on Nosto as well as show stale data when it comes to personalisation, and rules depending on affinity/segments \
