# Custom Storefronts (Hydrogen, Headless & more)

Personalization in Nosto depends on the ability to recognize returning visitors, follow them through the purchase journey, and attribute purchases correctly. On Shopify, tracking and session persistence require a few special building blocks — especially during Checkout. If your store is built using Shopify Hydrogen, Next.js, or another custom frontend framework, session tracking requires a slightly different setup than it does with standard storefronts.&#x20;

This guide explains how to keep Nosto Personalization working across the customer journey — even when the standard tracking cookie and Shopify Pixel don’t work out of the box.

## Why Custom Setups Are Different

In custom storefronts, Shopify often hosts the Storefront and Checkout on **different domains** (e.g. yourstore.com and checkout.shopify.com). Because of this, browser cookies (like Nosto’s 2c.cId) don’t automatically carry over from one context to the other.

**As a result:**&#x20;

* The cookie is not available in checkout
* The Shopify Pixel may not trigger
* Session continuity can break without manual intervention

## What’s Required for Tracking to Work

To maintain session tracking across Storefront & Checkout, you’ll need to:

* Ensure the Nosto script runs on the custom frontend
* Manually sync session data with Nosto before Checkout

### Manual Session Token Sync (Required)

You’ll need to send the customer’s session ID, cart token, and shop ID to Nosto before redirecting to checkout. You can use this function:

```javascript
export async function checkTokenToCustomer(cartIdGid, shopIdGid, providedCustomerId) {
  let customerId = providedCustomerId || getCookie('2c.cId');
  const cartToken = cartIdGid?.replace('gid://shopify/Cart/', '');
  const shopId = shopIdGid?.replace('gid://shopify/Shop/', 'shopify-');

  if (!customerId || !cartToken || !shopId) return;

  const NOSTO_URL = `https://connect.nosto.com/token/${shopId}/${customerId}/${cartToken}`;
  try {
    await fetch(NOSTO_URL);
  } catch (err) {
    console.error('Nosto: error updating cart:', err);
  }
}
```

💡 Call this after cart updates and right before sending the user to Checkout.

