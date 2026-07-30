---
description: >-
  Feature available in Nosto Plugin ≥ 3.5.5 (Shopware 6.5), ≥ 5.1.5 (Shopware
  6.6) and ≥ 6.6.0  Purpose Respect the Visibility per Sales Channel flag in
  Shopware so that hidden products are automaticall
---

# Product Visibility

### 1 Prerequisites

| Component        | Minimum version                                |
| ---------------- | ---------------------------------------------- |
| **Nosto Plugin** | 3.5.5 (SW 6.5) · 5.1.5 (SW 6.6) · 6.0.0 (6.7+) |
| **Shopware**     | 6.5.0 or later                                 |

In **Shopware Admin → Catalogues → Products** each product now exposes a **Visibility** toggle per sales channel:

<figure><img src="../../.gitbook/assets/Screenshot from 2025-06-09 09-51-58.png" alt=""><figcaption></figcaption></figure>

Earlier plugin releases ignored this flag. The new versions filter products based on it—after a one-time setup.

***

### 2 One-Time Setup

#### Step 1 Run a Full Product Sync

```
Shopware Admin → Marketing → Nosto Job Listing → Schedule Full Product Sync
```

Wait until the job status shows **Finished**.

***

#### Step 2 Index Custom Fields in Nosto

1.  **MyNosto → Product Experience Cloud → Search → Settings → Indexed Fields**\
    _(You can do the same via Category Merchandising; once is enough.)_&#x20;

    <figure><img src="../../.gitbook/assets/Screenshot from 2025-06-09 09-57-59.png" alt=""><figcaption></figcaption></figure>
2. Click **Add attribute** twice and create the following custom fields:

| Field          | Type    | Purpose                                |
| -------------- | ------- | -------------------------------------- |
| `showsearch`   | Boolean | Visibility flag for Search API calls   |
| `showcategory` | Boolean | Visibility flag for Category API calls |

<figure><img src="../../.gitbook/assets/Screenshot from 2025-06-09 10-00-59.png" alt=""><figcaption></figcaption></figure>

***

#### Step 3 Enable the Feature Flag in the Plugin

> **Wait for indexing** — Nosto re-indexes every six hours. To verify completion you can temporarily create a facet:

* **MyNosto → Search → Settings → Facet Manager → New facet group**
* Pick `showsearch` (or `showcategory`).\
  _&#x49;f the “attribute unavailable” warning disappears, indexing is done. Delete the test facet._

Once indexing is confirmed:

```
Shopware Admin → Extensions → My Extensions → Nosto → Configuration → Enable products visibility
```

<figure><img src="../../.gitbook/assets/Screenshot from 2025-06-09 10-09-28.png" alt=""><figcaption></figcaption></figure>

***

### 3 Result

After these three steps the Nosto plugin automatically appends `showsearch=true` or `showcategory=true` filters in its Search and Category API calls. Products hidden for a given sales channel in Shopware will no longer surface in Nosto recommendations, merchandising rules, or search results for that channel.

***

#### Need Help?

_Verify that:_

1. **Full Product Sync** completed without errors.
2. Both custom fields appear under **Indexed Fields**.
3. The **Enable products visibility** flag is switched **on**.
4. Plugin version meets or exceeds the minimum listed above.

