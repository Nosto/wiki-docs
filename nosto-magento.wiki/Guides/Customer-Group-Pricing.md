Nosto supports pricing for customer-groups. Using the customer-groups feature allows you to change the price and availability of products in recommendations depending upon the customer's group.

![3.2.0](https://img.shields.io/badge/nosto-3.2.0-green.svg)

**Note:** At the time of the writing, it isn't possible to use exchange-rates base multi-currency in conjunction with the customer-group pricing. They are mutually exclusive.

The most common use case for customer-groups is for selling to other businesses. Typically, store's selling to businesses requires the user to be logged in. Once logged in, the store shows B2B prices, while normal customers see the retail price.

By default, Magento ships with a few canned customer groups as shown below

![](https://user-images.githubusercontent.com/327432/31808213-d358ee30-b57a-11e7-8995-9820be4dd9ba.png)

## Enabling / Disabling Customer-Group Pricing

The customer-group pricing is disabled by default. You can toggle the customer-group pricing tagging by navigating to the configuration page and toggling **Enabled** under the **Price Variation** section.

_Note: You will need to request a full-reindex of your product catalog once you have toggled this feature on or off. You may also need to invalidate your full-page cache (FPC). Please contact support@nosto.com for the reindex request._

![](https://user-images.githubusercontent.com/327432/31808062-47d7a194-b57a-11e7-8cb2-5b3ec46582e9.png)

## Verifying

Once you've enabled the customer-pricing group, you should be able to see the new product metadata collected by Nosto. 

![](https://user-images.githubusercontent.com/327432/31809630-42a24cea-b581-11e7-82d7-815ace2ff86e.png)
