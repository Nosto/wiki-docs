# Using Yotpo

Nosto for Magento supports ratings and reviews powered by [Yotpo](https://marketplace.magento.com/yotpo-module-yotpo.html). In order to begin leveraging Yotpo ratings and reviews in Nosto, you'll need to install [Yotpo's extension for Magento 2](https://github.com/YotpoLtd/magento2-plugin) \(version 2.4\).

![2.15.1-beta](https://img.shields.io/badge/nosto-2.15.1-red.svg)

![](https://www.yotpo.com/wp-content/uploads/2017/01/nosto_Integration_logos.png)

## Configuring Yotpo for Magento

Once you've installed the Yotpo for Magento extension, you'll need to follow their guide on [setting up "rich snippets" in Magento](https://support.yotpo.com/en/article/magento-2-installing-yotpo)

## Configuring Nosto to use Yotpo

You can switch the rating and reviews metadata provider to be Yotpo by navigating to the configuration page and toggling **Enable Ratings & Reviews** to **Use Yotpo** under the **Feature Flags** section.

**Note:** the **Use Yotpo** option is only visible once Yotpo's extension for Magento is installed.

![Verifying Rating &amp; Review Tagging](https://user-images.githubusercontent.com/44775916/48712439-6ef84480-ec16-11e8-90c5-4a9730d1a504.png)

## Verifying

Once you've enabled the rating and reviews metadata, you should be able to see the new product metadata collected by Nosto.

![Verifying Rating &amp; Review Tagging](https://user-images.githubusercontent.com/44775916/48712817-65231100-ec17-11e8-91f4-ce53681c8d78.png)

## Yoptpo reviews and ratings are cached for 24 hours.

Yotpo reviews and ratings are cached for 24 hours in Magento. This means that the review count and rating values are not updated to Nosto immediately after new reviews are posted

