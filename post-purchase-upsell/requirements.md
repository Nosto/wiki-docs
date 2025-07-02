---
description: Key Requirements for using Post-Purchase Upsell.
---

# Requirements

### Nosto Setup

* [Nosto must be installed](../Installing.md) and active.
* [Tracking & Session management](../tracking-and-session-management/) must be in place
* Product catalog must be synced.
* Post Purchase Upsell module must be active.

### Shopify Setup

* Nosto must be selected as "Post-purchase page" app in Shopify Checkout Settings

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption><p>Nosto must be selected as "Post-purchase page" app in Shopify Checkout Settings</p></figcaption></figure>

### Main Currency Only

Shopify does not support post-purchase upsell in secondary currencies via Shopify Markets. Offers will only be shown if the purchase was made in the store’s main currency.

## Limitations to Be Aware Of

Some scenarios may block the upsell page from showing. Common causes include:

* Unsupported Payment Provider
* Order paid fully with a gift card
* Use of unsupported Payment Methods
* Only works for stores **default currency**

{% hint style="info" %}
Please make sure, to align Post Purchase Upsell with your fulfillment rules, and ensure it follows Shopifys order status.&#x20;
{% endhint %}

## **For more, refer to**

\[[Requirements and Shopify Limitations](https://help.nosto.com/en/articles/11530627-requirements-and-shopify-limitations)]\
\[[Handling Partially Paid Post Purchase Orders](https://help.nosto.com/en/articles/11533532-handling-partially-paid-post-purchase-orders)]
