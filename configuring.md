# Configuring

## Nosto Account Setup

The extension creates a new menu item to the admin top menu during installation. Note that you may have to clear the cache for the menu item to show up.

By clicking the menu item, you will be redirected to the Nosto account configuration page were you can create and manage your Nosto accounts. You will need a Nosto account for each store view in the installation.

![screen shot 2018-04-09 at 13 47 12](https://user-images.githubusercontent.com/2778820/38494117-5de7fdf8-3bfd-11e8-86b2-45e10a0cf806.png)

Creating the account is as easy as clicking the install button on the page. Note the email field above it. You will need to enter your own email to be able to activate your account. After clicking install, the window will refresh and show the account configuration.

You can also connect and existing Nosto account to a store, by using the link below the install button. This will take you to Nosto where you choose the account to connect, and you will then be redirected back where you will see the same configuration screen as when having created a new account.

You should now be able to view the default recommendations in your stores frontend by clicking the preview button on the page.

![screen shot 2018-04-09 at 13 52 28](https://user-images.githubusercontent.com/2778820/38494118-5e042172-3bfd-11e8-980e-a1f5b65d40a6.png)

## Advanced Configuration

There is also an advanced configuration section for the extension. It can be found under "System -&gt; Configuration -&gt; Services -&gt; Nosto". Here you have some options for changing the extension behaviour:

### Image version

This option determines which of the Magento image types is to be used for the product images in Nosto. By default, it is the "Base Image".

![](https://cloud.githubusercontent.com/assets/327432/19802512/bf6b9526-9d0c-11e6-8555-c076aef5ab21.png)

This panel has tree image version to choose from:

* **Base Image**: The base image is the image version used as the main image on the product detail page.
* **Small Image**: The small image is the image version used as in the product listings such as category pages and search results.
* **Thumbnail**: The thumbnail is the image version used as the small image in shopping carts such the cart page or sidebar cart implementations

The extension always fetches the original version of the image and not a resized one. Nosto stores a copy of the original image and resizes, crops and saves the thumbnail to it's CDN.

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Brand Attribute

This option determines the Magento EAV attribute whose value will be used as the Brand attribute.

This dropdown will list all the attributes available. Choose the appropriate one and click "Save". You can validate that the change has taken effect by navigating a product page and using the Nosto Debug Tool to check the value of the Brand attribute.

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### URL Configuration

This option removes the trailing `___store` parameter from the product URLs. If you have only a single-store view or would like to enable clean URLs, this can be toggled on.

![pasted image at 2017\_06\_19 12\_52 pm 1](https://user-images.githubusercontent.com/327432/27281480-bae4f9c0-54f4-11e7-8143-f20869757b22.png)

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Advanced Settings

![image](https://user-images.githubusercontent.com/2778820/56135456-76d7cd00-5f99-11e9-9bbc-9976e00ac4fd.png)

#### Send Custom Fields

This option enables sending product attributes as custom fields to Nosto. The information is present on the tagging and in the page source, but hidden. If this option is disabled, the information is removed from the page source code.

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

#### Enable Customer Data Sending

This option allows the merchant to enable/disable the sending of customer data to Nosto. If this option is disabled no customer tagging is done by Nosto.

#### Indexer Memory

From version `3.9.0` you can now limit the amount of PHP memory available for the Nosto indexer process. Having a large catalog you may increase this value to allow the process to be completed.

#### Add product published date to tagging

Set this to `Yes` if you want to send the date a product has been added to Magento's catalog to Nosto.

### Rating And Reviews

The rating and review dropdown allows you to select the rating and review provider. For more information on leveraging this feature, we recommend reading our guide on [ratings and reviews](features/ratings-and-reviews.md).

![screen shot 2018-04-06 at 15 40 50](https://user-images.githubusercontent.com/2778820/38488300-b85a0496-3beb-11e8-90cb-b6e82c0389b1.png)

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Scheduled Currency Rate Update

![screen shot 2018-04-06 at 15 40 58](https://user-images.githubusercontent.com/2778820/38488306-c1a52fd0-3beb-11e8-8cd9-ca55c1251285.png)

### Price Variation

The price-variation dropdown allows you to enable support the customer group pricing and catalog price rules. For more information on using price variations, we recommend reading our guide on [customer group pricing and catalog price rules](features/customer-group-pricing.md).

![screen shot 2018-04-06 at 15 41 11](https://user-images.githubusercontent.com/2778820/38488307-c1c4b760-3beb-11e8-9b13-33e26c9b197b.png)

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Multi Currency

The multi-currency dropdown allows you to toggle the multi-currency feature. For more information on using multiple currencies, we recommend reading our guide on [multi-currency \(exchange-rates\)](features/multi-currency-exchange-rates.md).

![screen shot 2018-04-06 at 15 41 18](https://user-images.githubusercontent.com/2778820/38488308-c1e573d8-3beb-11e8-96c6-c8d1cde89934.png)

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Optional Attributes

The optional attributes drop-downs allow you to select the EAV that will be used for the brand and the GTIN. Both the brand and GTIN fields are simple Magento attributes and therefore need to be manually selected.  


![](.gitbook/assets/image.png)

From version `4.1.0` Nosto extension also supports tagging Google category attribute. Similarly to GTIN attribute described above, select the attribute you wish to map using the selector and confirm on the product tagging using the debug toolbar.

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Attributes To Tags

The attribute-to-tag mapper can be used to used to map certain Magento EAVs to Nosto's tags and custom fields. By default, Nosto maps all visible product EAVs to custom fields but this selector gives you granular control on which attributes are mapped.

![screen shot 2018-04-06 at 15 41 43](https://user-images.githubusercontent.com/2778820/38488311-c2225276-3beb-11e8-8b0e-3eb6b5143b36.png)

**Note:** A full [reindex of your store](https://help.nosto.com/settings-and-troubleshooting-faq/tools-product-reindexupdate) is required after this configuration is changed.

### Currency Formats

The currency format section shows you all the defined currency formats in Magento. When recommendations are shown, Nosto will format all prices according to these format specifications.

![screen shot 2018-04-06 at 15 41 59](https://user-images.githubusercontent.com/2778820/38488312-c23f9516-3beb-11e8-9e80-3d91b9711c18.png)

