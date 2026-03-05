---
description: >-
  How to control PLP & SERP filters when your store uses Nosto-powered SERP and
  PLP
---

# Filters

### 1 Why facets come from Nosto, not Shopware

Once Nosto is the data source for a Product-Listing Page or Search-Result Page, Shopware no longer controls the filter sidebar. Instead, every available filter (facet) is read from Nosto’s Search index.\
This means:

| Behaviour                           | Explanation                                                  |
| ----------------------------------- | ------------------------------------------------------------ |
| **Facet list**                      | Only attributes indexed in Nosto are shown.                  |
| **Facet order, labels & hierarchy** | Managed in Nosto Admin.                                      |
| **Adding a new filter**             | Create it in Nosto (Shopware settings alone have no effect). |

**Note on customization support**

> The visual appearance and frontend behavior of filters (for example styling, layout, or UI modifications) are part of the merchant’s storefront implementation. Any customizations to the look and feel of filters are not supported by Nosto and should be implemented by the merchant or their development team.

***

### 2 Best-practice tips

| Tip                                      | Why it matters                                                                  |
| ---------------------------------------- | ------------------------------------------------------------------------------- |
| **Keep the number of facets manageable** | Too many filters overwhelm customers and slow down page rendering.              |
| **Use descriptive labels**               | Customers see the facet label, not the technical field name.                    |
| **Group related facets**                 | E.g. place “Size” & “Fit” under a “Clothing” facet group for clarity.           |
| **Avoid indexing rarely-used fields**    | Every indexed field adds to search payload size.                                |
| **Re-order facets based on analytics**   | Nosto’s interface lets you drag & drop facets to prioritise high-usage filters. |

***

### 3 Troubleshooting

| Symptom                                   | Checklist                                                                                                                              |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Attribute missing from facet dropdown     | <p>• Confirm it’s filled for at least one product in Shopware.<br>• Run a full sync.<br>• Wait for indexing to finish (up to 6 h).</p> |
| Facet appears but shows “0” product count | <p>• Products may lack values for that attribute.<br>• Attribute type might be set incorrectly (e.g. List vs Text).</p>                |
| Filters vanished                          | <p>• Ensure Nosto is still data source (plugin enabled).<br>• Verify the <em>Enable Cache</em> interval isn’t masking fresh data.</p>  |

***

### 4 Further reading

* **Nosto Help Center** “Setting up facets” – [https://help.nosto.com/en/articles/7169091-setting-up-facets](https://help.nosto.com/en/articles/7169091-setting-up-facets)
* **Shopware Docs** Custom Fields & Properties – https://docs.shopware.com (search: “custom fields”)

***

####
