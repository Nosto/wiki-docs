# Tracking Events

## Upon viewing the homepage

When viewing a home-page, there's no context to be provided, so invoking the `viewIndex` will suffice.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewFrontPage()
    .setPlacements(['homepage-nosto-1', 'bestseller-recs'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

## Upon viewing a product

When viewing a product, you should send the product-id of the current product being viewed. Unlike the regular implementation, you do not need to pass the entirety of the product metadata.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewProduct('product-id')
    .setPlacements(['product-crosssells'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

## Upon viewing a collection

When viewing a category or collection, you should send the slash-delimited and fully-qualified path of the current category.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewCategory('/Womens/Dresses')
    .setPlacements(['category-related'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

**Note:** You don’t need to ensure the case-sensitivity of the category being passed so long as the path is tagged in the same way as your product’s categories are.

## Tagging the categories

Categories must always be delimited by a slash. For example, `/Home/Accessories` is a valid category while `Home > Accessories` is not.

## Upon doing a search

When viewing the results of a search, you must send the exact search-term as queried for.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewSearch('womens dresses')
    .setPlacements(['search-related'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

**Note:** You don’t need to normalize or encode the search query in any way.

## Upon starting a checkout

When viewing a checkout page, there's no context to be provided, so invoking the `viewCheckout` will suffice.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewCart()
    .setPlacements(['cart-related'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

## Upon placing an order

On all thank-you and order-confirmation views, the order confirmation metadata _must_ be passed.

The order confirmation metadata is used for sending personalised order-followup emails, personalise the recommendations e.g. order-related, for segmentation insights and conversion statistics.

**Important** Even if you would not display any recommendations in your order-confirmation view you must still set placements \(`.setPlacements(...)`\) and load \(`.load()`\) the results. Setting the order works in a similar manner than [cart](spa-basics-managing-sessions.md#setting-the-cart) and [customer](spa-basics-managing-sessions.md#setting-the-customer) and an [action](session-api-terminology.md#action) must be performed for the data to be sent to Nosto.

```javascript
nostojs(api => {
  api.defaultSession()
    .addOrder(
      {
        "external_order_ref": "145000006",
        "info": {
          "order_number": "195",
          "email": "mridang@nosto.com",
          "first_name": "Mridang",
          "last_name": "Agarwalla",
          "type": "order",
          "newsletter": true
      },
      "items": [
        {
          "product_id": "406",
          "sku_id": "243",
          "name": "Linen Blazer (White, S)",
          "quantity": 1,
          "unit_price": 455,
          "price_currency_code": "EUR"
        }
      ]
    })
    .setPlacements(['order-related'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

## Upon viewing a page that was not found \(404\)

When viewing a page / view that was not found, there's no context to be provided, so invoking the `viewNotFound` will suffice.

```javascript
nostojs(api => {
  api.defaultSession()
    .viewNotFound()
    .setPlacements(['notfound-nosto-1', 'bestseller-recs'])
    .load()
    .then(data => {
      console.log(data.recommendations);
    })
});
```

