# Updating Products

While Nosto's crawler attempts to keep its copy of your catalog as fresh as possible, there are scenarios where we may not be able to update all the information as quickly as needed.

In these scenarios, we recommend that you implement our enhanced Products API which allows you push all your product metadata and have the changes instantly reflect across our entire engine.

For example, if you add a discount of -10% to all the products in your "Shirts" category, you can use the Products API to bulk update all the products.

Here's an example of a Curl request.

```text
curl -v --user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw -H "Content-Type: application/json" -X POST https://api.nosto.com/v1/products/upsert -d '
[
  {
    "url":"http://shop.example.com/product/123",
    "product_id":"123",
    "name":"Example product 123",
    "image_url":"http://cdn.example.com/product_123.jpg",
    "price_currency_code":"EUR",
    "availability":"InStock",
    "rating_value":"3.8",
    "review_count":"15",
    "categories":[
      "sale/summer",
      "shirts"
    ],
    "description":"Example description",
    "price":10.00,
    "list_price":12.34,
    "brand":"Example Brand",
    "tag1":[
      "red",
      "green"
    ],
    "tag2":[
      "women"
    ],
    "tag3":[
      "Foldable"
    ],
    "date_published":"2013-­04-­23",
    "variation_id":"EUR_1",
    "variations":{
      "USD_2":{
        "price_currency_code":"USD",
        "availability":"InStock",
        "price":12.00,
        "list_price":15.67
      },
      "GBP_3":{
        "price_currency_code":"GBP",
        "availability":"OutOfStock",
        "price":20.00,
        "list_price":21.99
      }
    },
    "inventory_level":25,
    "supplier_cost":1312.96,
    "custom_fields":{
      "material":"Cotton",
      "weather":"Summer"
    },
    "skus":[
      {
        "id":"2",
        "name":"S-Blue",
        "price":1269.0,
        "list_price":1299.0,
        "url":"http://www.example.com/product/CANOE123#/1-size-s/14-color-blue",
        "image_url":"http://www.example.com/product/images/CANOE123-1.jpg",
        "availability":"InStock",
        "custom_fields":{
          "size":"S",
          "color":"Blue"
        }
      },
      {
        "id":"1",
        "name":"S-Orange",
        "price":1269.0,
        "list_price":1299.0,
        "url":"http://www.example.com/product/CANOE123#/1-size-s/13-color-orange",
        "image_url":"http://www.example.com/product/images/CANOE123-1.jpg",
        "availability":"InStock",
        "custom_fields":{
          "size":"S",
          "color":"Orange"
        }
      }
    ]
  }
]'
```

## How can I get an API token?

You can request an API token \(API\_PRODUCTS\) by getting in touch with our support personnel. Once the token has been granted, you will be able to find it listed in the [authentication tokens section in the admin.](https://help.nosto.com/settings-and-troubleshooting-faq/settings-authentication-tokens)

## How many items can I update at a time?

The Products API takes an array of product metadata and has no hard limit on the number of items that you can specify and is only limited by the maximum size of the payload of 2 MB.

## How often can I update products?

You can update products as often as you need although, initially you'll need to send the entire catalog to populate the replica of your catalog on Nosto.

## When should I make an API call?

You should make an API call when any product information changes. If a non-critical field of the product e.g. description, has changed, you can delay the product update as descriptions are rarely visible to the end customer. When the price or availability of the product changes, you should make an API call right away as this will instantly reflect in the recommendations.

## Is there any additional benefit of using the product API?

While the product API contains the full superset of the information in the tagging, it also allows you to pass sensitive information to us. Sensitive information is fields such as the supplier cost \(margin\) and the inventory level. These fields are only mutable via the API.

