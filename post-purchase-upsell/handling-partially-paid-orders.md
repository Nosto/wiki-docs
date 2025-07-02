---
description: >-
  Understand how partially paid orders can occur with post-purchase upsell, how
  Shopify handles them, and what you should do to prevent fulfillment issues.
---

# Handling Partially Paid Orders

## Why Partially Paid Orders Happen

Post Purchase Upsell is builded as native Shopify Extension. Once a customer completes payment, Shopify places the order on hold, and gives Nosto the opportunity to show a final offer. The upsell is charged to the original payment method.

If something fails in this final step (e.g. there are not enough funds on customers payment method provided), the order might still be created, but only `partially paid`.

## Common causes

* The customer accepts an upsell offer, but the payment can’t be completed successfully.
* The original order was fully paid, but the upsell item is added without a successful follow-up charge.
* Potential issues with unsupported payment providers

## What Shopify Does

* The order is **marked as Partially Paid** in Shopify’s Admin.
* **Shopify sends a payment request email** to the customer (if configured).

This is **standard Shopify behavior** and not controlled by Nosto.

## What You Could Do

It is your responsibility to **prevent fulfillment of unpaid items** and ensure operational safety. Still, we want to share the most common ways we see for these orders to be handled:

### **1. Review your fulfillment conditions**

Ensure your system respects this order status type, and does not automatically fulfill partially paid orders.&#x20;

**In Shopify Admin, check:**

* Order status settings
* Third-party fulfillment or ERP/WMS logic
* Manual vs. automatic fulfillment triggers

### **2. Set Up Rules**

You can set up automation to send Partially Paid orders to `Manual Review` using e.g. Shopify Flow. This allows you to pause all following steps, and to review the orders first. From there you can decide to:

1. **Collect payment for the unpaid item**
   * Shopify automatically sends an email to the customer with a secure payment link.
   * You can also manually resend the payment request from the order view in Shopify.
2. **Remove the unpaid item manually**\
   You can edit the order in Shopify and remove the upsell product. This restores the order status to Paid and ensures only paid products go to fulfillment.



\


