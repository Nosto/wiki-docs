# FAQ

## How does Nosto treat tax settings?

As of version 2.10.3 Nosto extension tags the products according to the store's Price Display Settings \(see screenshot\) below. ![configuration\_\_\_settings\_\_\_stores\_\_\_magento\_admin](https://user-images.githubusercontent.com/15191701/40839884-0825538c-65ad-11e8-9e77-14445b42b877.png)

If the value of this setting is "Including Tax" Nosto will use the prices with taxes. If the value is "Excluding Tax" or "Including and Excluding Tax" Nosto will use prices without taxes.

## How do I enable or disable multi-currency?

See article about [Multi Currency \(Exchange Rates\)](features/multi-currency-exchange-rates.md)

## How to improve order confirmation performance?

Please follow the following steps: 1. Update to latest nosto version: [https://github.com/Nosto/nosto-magento2/releases](https://github.com/Nosto/nosto-magento2/releases) 2. Setup cron job for magento. [http://devdocs.magento.com/guides/v2.1/config-guide/cli/config-cli-subcommands-cron.html](http://devdocs.magento.com/guides/v2.1/config-guide/cli/config-cli-subcommands-cron.html) 3. Change nosto indexer mode to Update by schedule. All indexers and their mode \(including Nosto indexer\) can be found from store admin “System” &gt; “Index management”. [https://user-images.githubusercontent.com/15191701/29363535-0f27b212-8299-11e7-9f31-ff9b5dd9a922.png](https://user-images.githubusercontent.com/15191701/29363535-0f27b212-8299-11e7-9f31-ff9b5dd9a922.png)

That improves the order confirmation performance and backend operation performance. But please make sure the magento cron job is running properly, otherwise nosto product information will not be updated to nosto in time.

## How to deal with failing customer reference creation due to large customer pool

When the module is installed or upgraded it will generate a customer_reference for every customer in the customer_entity table. Currently, the module makes use of the customer collection, but this can be a costly operation in case of a big customer pool. 

Execute the following SQL query to generate the attribute for each of your customers. This will resolve the failing installation.

```mysql
INSERT INTO magento2.customer_entity_varchar 
            (attribute_id, 
             entity_id, 
             value) 
SELECT (SELECT attribute_id 
        FROM   magento2.eav_attribute 
        WHERE  attribute_code = "nosto_customer_reference"), 
       entity_id, 
       SHA2(Concat(entity_id, email), 256) 
FROM   magento2.customer_entity
```
## How do I remove the `___store` parameter from the URLs?

In order to remove the `___store` parameter, you will need to enable clean-urls in the advanced configuration. Please see the [URL Options](configuring.md#url-options) section in our configuration guide.

## Why is incomplete / cancelled order attributed to Nosto

Nosto's Magento extension send information about an order to Nosto via tagging and via API. Order is sent over the API to Nosto whenever an order \(`Magento\Sales\Model\Order`\) is saved in Magento 2. The definition of the observer can be [from here](https://github.com/Nosto/nosto-magento2/blob/master/etc/events.xml#L40-L42). This means that order data gets sent to Nosto when the order is created or updated regardless of the status of the order. Nosto has pre-defined set of order statuses that are automatically ignored \(cancelled, rejected, etc.\). If the initial order status or order updates sent within 30 minutes of the purchase don't have status that is considered ignored the orders will be treated as valid in Nosto.

If a merchant provides a possibility for offline payments \(cash on delivery, invoice, cheque, etc.\) or 3rd party payment gateways that do not redirect back to the store Nosto has no way of knowing if the payment has been fulfilled or not. These orders are also considered as valid unless they are cancelled within that 30 minutes window.

More info about the attribution can be found from [here \(Nosto's payment terms\)](http://www.nosto.com/payment-terms/)

## How to disable recommendation autoloading

The minimum module version required for this feature is `3.6.1`. If a merchant does not prefer to fetch the product recommendation data when the page is loaded, the autoload can be set to `false`. The solution is to fetch the product recommendation when the user scroll down to the slot.

This is done by extending our module following our docs: [Overriding or extending functionalities](guides/overriding-or-extending-functionalities/)

The logic followed is by adding an interceptor to our class, more info on interceptors can be found on [Magento documentation](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/plugins.html)

Remember that you still need to call `nostojs(function(api){api.loadRecommendations()})` when you want the recommendations to be loaded and populated.

### Required files and configurations:

#### 1. `app/code/My/Nosto/Block/Stub.php`

```php
<?php

namespace My\Nosto\Block;

use Nosto\Nosto;
use Nosto\Tagging\Block\Stub as NostoStub;

class Stub
{
    /**
     * Method called after isRecoAutoloadDisabled() is executed
     * in order to override the return value
     * 
     * @param NostoStub $stub
     * @return bool
     */
    public function afterIsRecoAutoloadDisabled(NostoStub $stub)
    {
        return true;
    }
}
```

#### 2. `app/code/My/Nosto/etc/di.xml`

```markup
<config>
    <type name="Nosto\Tagging\Block\Stub">
        <plugin name="my_nosto_stub_plugin" type="My\Nosto\Block\Stub" sortOrder="1" disabled="false" />
    </type>
</config>
```

Run the following commands to update the generated code in Magento's end

`bin/magento setup:update`

`bin/magento setup:di:compile`

## How to reconnect Nosto account

Sometimes you might need to reconnect a Nosto account to your store view. This could be due to for example expired or missing tokens.

First you must [disconnect](disconnecting-nosto-from-store-front.md) the existing Nosto account from your store.

After you have disconnected an account you can re-connect the account by following the [normal Oauth flow](getting-started.md#connecting-with-an-existing-nosto-account)

## Placeholder image for products without images

In case a product does not have any image, the Nosto module will use the configured placeholder as the image for the product.

You can setup placeholder images from Magento 2 admin under "Store > Configuration > Catalog -> Product Image Placeholders". 
"Base" image placeholder will be used as a placeholder image.

## Difference between 'Discontinued' and 'Invisible' products

A product's availability will be tagged as `Invisible` if its Visibility is set to `Not visible individually` on Magento. That does not include
SKUs which are tagged within the configurable product.

`Discontinued` products are products that no longer exist or cannot be tagged (e.g. if disabled).

By default, disabled products will not be tagged by Nosto or will end up as `Discontinued` if disabled after being tagged.
Starting from version `5.4.0` there is also a possibility to send disabled product data to Nosto via API.
The option can be enabled from Magento's Nosto settings (Stores > Configuration > Services > Nosto > Feature Flags > Index disabled products).‌
With the option enabled Nosto will tag disabled products as `Invisible`.
