# Managing Sessions

## Setting the cart

The cart content _must_ updated whenever the cart contents change. The cart contents are the 1:1 representation of the user's cart. Also note that the cart contents is only send to Nosto when you perform an [action](session-api-terminology.md#action). If you want to send the cart contents to Nosto right away, you add `.viewCart().update()` to your call. See full example below.

You may also pass `null` or `undefined` to signify that there was no change in the cart.

The cart information is used by the Nosto to tailor the recommendations, dispatch abandoned cart emails and fire Facebook pixel events for retargeting purposes.

```javascript
nostojs(api => {
  api.defaultSession()
   .setCart({
     items: [
      {
        name: "Men's Running Shirt",
        price_currency_code: "EUR",
        product_id: "181503",
        quantity: 2,
        sku_id: "181505",
        unit_price: 123.45
      },
      {
        name: "Men's Training Shoe",
        price_currency_code: "EUR",
        product_id: "34552",
        quantity: 1,
        sku_id: "39912",
        unit_price: 999.00
      }
    ]
   })
});
```

or when sending the contents to Nosto right away

```javascript
nostojs(api => {
  api.defaultSession()
   .setCart({
     items: [
      {
        name: "Men's Running Shirt",
        price_currency_code: "EUR",
        product_id: "181503",
        quantity: 2,
        sku_id: "181505",
        unit_price: 123.45
      },
      {
        name: "Men's Training Shoe",
        price_currency_code: "EUR",
        product_id: "34552",
        quantity: 1,
        sku_id: "39912",
        unit_price: 999.00
      }
    ]
   })
   .viewCart()
   .update()
});
```

**Note:** Passing `null` or `undefined` will prevent the cart contents from being mutated on Nosto. Passing an empty object `{}` will reset the cart contents.

## Setting the customer

When a visitor logs in customer information _should_ be passed. If the customer isn't logged in, this can be omitted. Similar to setting the [cart contents](../manual-implementation/cart-tagging.md), customer data is only sent to Nosto when an [action](session-api-terminology.md#action) is performed.

The customer information is primarily used for sending personalised triggered emails and for building multi-channel experiences.

```javascript
nostojs(api => {
  api.defaultSession()
    .setCustomer({
      customer_reference: "b369f1235cf4f08153c560.82515936",
      email: "devnull@nosto.com",
      first_name: "Nosto",
      last_name: "Test",
      newsletter: true
    })
});
```

**Note:** Passing `null` or `undefined` will prevent the customer information from being mutated on Nosto. Passing an empty object `{}` will reset the customer information.

### Tagging marketing permission

The new marketing-permission flag denotes whether the customer has consented to email marketing. If the marketing-permission field is omitted, we assume that the current customer has not given their consent and Nosto will refrain from sending out any personalized triggered emails.

The marketing permission is false by default but if a user has explicitly agreed to receive marketing then you can set it to true manually. In practice, this means reading and mapping the value from opt-in for marketing in your platform e.g. a consumer explicitly subscribed for marketing emails when checking out.

The marketing-permission should be included as a part of the customer tagging and should be rendered on all pages.

### Tagging customer reference

The customer-reference can be leveraged to unify sessions across channels such as between online and offline. It is a unique identifier provided by you that is used in conjunction with the Nosto cookie. The customer-reference can also be used to uniquely identify users in lieu of an email address.

The customer-reference should be a long, secure and a non-guessable identifier. For example, use your internal customer-id or the customer's loyalty program identifier and use a secure hash function like an HMAC-SHA256 to hash it.

