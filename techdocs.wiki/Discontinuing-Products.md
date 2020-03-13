While Nosto's crawler attempts to keep its copy of your catalog as fresh as possible, there are scenarios where we may not be able to update all the information as quickly as needed.

In these scenarios, we recommend that you implement our Discontinue API which allows you to push all your product identifiers to discontinue them and have the changed instantly reflect across our entire engine.

Here's an example of a Curl request.

```
curl -v --user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw -H "Content-Type: application/json" -X POST https://api.nosto.com/v1/products/discontinue -d '
[
  '101',
  '102' 
]'
```

### How can I get an API token?

You can request an API token (API_PRODUCTS) by getting in touch with our support personnel. Once the token has been granted, you will be able to find it listed in the [authentication tokens section in the admin.](https://help.nosto.com/settings-and-troubleshooting-faq/settings-authentication-tokens)

### How many items can I update at a time?

The Discontinue API takes an array of product metadata and has no hard limit on the number of items that you can specify and is only limited by the maximum size of the payload of 2 MB.

### How often can I discontinue products?

You can discontinue products as often as you need.

### When should I make an API call?

You should make an API call when any product is discontinued and removed from your catalog. If a product has simply gone out of stock, we recommend making an API to update the product with the correct availability.

### Is there any additional benefit of using the Discontinue API?

While Nosto crawls your site and automatically discontinues products that are no longer available, it may lag behind when your site has a large catalog. In these environments, it is recommended to use the Discontinue API to immediately discontinue the product.