---
description: >-
  Introduction to Nosto’s Post Purchase Upsell extension for Shopify - what it
  does, how it works, and what you need to enable it.
---

# Overview & Requirements

## &#x20;What is Post Purchase Upsell?

Nosto’s Post Purchase Upsell lets you present one last personalized product offer right after a customer completes payment but before they reach the Shopify Thank You page. This is done using Shopify’s Post Purchase Extensions framework, ensuring a seamless checkout flow with no redirects or popups.

The extension allows merchants to:

* Increase average order value with zero friction.
* Control the offer logic and experience via Nosto.
* Rely on Shopify to handle order editing and charging the payment method.

## How It Works in Shopify

1. Customer completes payment → Shopify creates the order and sets it to **On hold** status.
2. Nosto’s Post Purchase Upsell offer page is displayed inside the Shopify Checkout.
3. Customer accepts or declines the offer:
   * **If accepted**: Shopify edits the order and charges the payment method.
   * **If declined**: order continues to fulfillment.
4. Once the step completes, Shopify releases the order from **On hold** status.

## Key Requirements

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
