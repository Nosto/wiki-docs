# Customer Group Pricing

Nosto supports pricing for customer-groups. Using the customer-groups feature allows you to change the price and availability of products in recommendations depending upon the customer's group.

![2.14.0](https://img.shields.io/badge/nosto-2.14.0-red.svg)

**Note:** It isn't possible to use exchange-rates base multi-currency in conjunction with the customer-group pricing. They are mutually exclusive. Also note that the price variations are not supported in SKUs \(configurable product variations\).

The most common use case for customer-groups is for selling to other businesses. Typically, store's selling to businesses requires the user to be logged in. Once logged in, the store shows B2B prices, while normal customers see the retail price.

By default, Magento 2 ships with a few canned customer groups as shown below

![image](https://user-images.githubusercontent.com/2778820/47842852-83d77a00-ddc6-11e8-9d2f-25593bea2a42.png)

## Enabling / Disabling Customer-Group Pricing

The customer-group pricing is disabled by default. You can toggle the customer-group pricing tagging by navigating to the Nosto configuration page and toggling **Yes** for **Price Variation** under the **Currency Setup** section.

_Note:_ You need to disable Multi-Currency in order to Price Variation selection appear.

_Note:_ You will need to request a full-reindex of your product catalog once you have toggled this feature on or off. You may also need to invalidate your full-page cache \(FPC\). Please contact support@nosto.com for the reindex request.

![image](https://user-images.githubusercontent.com/2778820/47842898-a5d0fc80-ddc6-11e8-85cb-f61fc29017d4.png)

## Verifying

Once you've enabled the customer-pricing group, you should be able to see the new product metadata collected by Nosto.

![image](https://user-images.githubusercontent.com/2778820/47843614-d9ad2180-ddc8-11e8-9419-bd668fb1742a.png)

