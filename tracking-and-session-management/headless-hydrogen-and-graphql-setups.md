# Headless, Hydrogen & GraphQL Setups

If you’re using Hydrogen or a custom storefront, Shopify may host the storefront and checkout on different domains (e.g. store.com vs checkout.shopify.com). This breaks the cookie and makes standard session tracking fail.

## Summary

* Nosto relies on the 2c.cId cookie for tracking
* Shopify blocks cookies in checkout, so the Shopify Pixel is required
* You must get cookie consent for the pixel to function
* Hydrogen and headless setups require **manual integration**

## How to make it work

You can manually send a token mapping request to Nosto before redirecting to checkout.

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

Call this on cart updates or just before navigating to checkout.

