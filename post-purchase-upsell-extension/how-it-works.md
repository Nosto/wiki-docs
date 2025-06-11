---
description: >-
  Understand what happens behind the scenes when a Post Purchase Upsell offer is
  triggered. Learn how the extension behaves within Shopify Checkout, and how
  Nosto processes and delivers the offer.
---

# How It Works

## How the Post Purchase Flow Works

When a customer completes a checkout in Shopify, the Post Purchase Extension is triggered immediately after the payment step. Here’s what happens step by step:

1. **Order is created**\
   Shopify creates the order and marks it with status `On hold`. This status prevents the order from moving to fulfillment while the post-purchase extension is active.
2. **Nosto is called**\
   Shopify loads the Nosto Post Purchase Extension. Nosto uses the order information and customer context to run your campaign logic.
3. **Offer page is rendered**\
   **—** If a matching offer is returned (based on product availability, filters, segment, etc.), the offer page is shown directly inside the Shopify Checkout.\
   — If no products are matching or if anything goes wrong, this step is ski**pped automatically.**
4. **Customer interacts**
   * If the offer is accepted:
     * The product is added to the order
     * The customer is charged using the same payment method
     * Shopify updates the order and removes the `On hold` status.
   * If the offer is declined **or** if the timer runs out:
     * The customer continues to the Thank You page
     * Shopify also removes the `On hold` status.
5. **No interaction / customer leaves the page**
   * Shopify waits **up to 1 hour**
   * Afterward&#x73;**,** the order is automatically resumed
   * Order status is updated accordingly

## What’s Handled by Nosto vs Shopify

| Offer logic & personalization | Nosto   |
| ----------------------------- | ------- |
| Segmentation logic            | Nosto   |
| Discount application          | Nosto   |
| Payment processing            | Shopify |
| Order editing (add item)      | Shopify |
| Order status management       | Shopify |

## Timeouts & Edge Cases

* The offer page is only active once per checkout and during timer duration – customers cannot return to it later.
* Shopify auto-resumes the order after 60 minutes if the customer closes the page.
* If Nosto doesn’t return any valid products, the post-purchase step is skipped entirely.

