In order add more custom attributes to Nosto SKU (aka variations) or to modify Nosto SKU data in general you must create a small Magento 2 module. By creating a M2 module you can override the SKU model in a way that the modifications take place in API calls, exports and in the tagging. The extension requires only 5 files to work.

If you already have a module that customizes Nosto product data or some other parts of Nosto module you don't need to create separate module and you only need to implement points 2. and 3. from instructions below.

First create following a directory / path: `app/code/My/Nosto`. Note that there's no need for channel (core, community or local) anymore in Magento 2.

## Required files and configurations

##### 1. The module config `app/code/My/Nosto/etc/module.xml`

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Module/etc/module.xsd">
    <module name="My_Nosto" setup_version="1.0.0" />
</config>
```

##### 2. The events config `app/code/My/Nosto/etc/events.xml`

```xml
<?xml version="1.0"?>
<!--suppress XmlUnboundNsPrefix -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Event/etc/events.xsd">
    <event name="nosto_sku_load_after">
        <observer name="nosto_sku_load_after" instance="My\Nosto\Observer\Product\Sku\Load" />
    </event>
</config>
```

##### 3. The observer `app/code/My/Nosto/Observer/Product/Sku/Load.php`

```php
<?php

namespace My\Nosto\Observer\Product\Sku;

use Magento\Catalog\Model\Product;
use Magento\Framework\Event\Observer;
use Magento\Framework\Event\ObserverInterface;
use Nosto\Object\Product\Sku;

class Load implements ObserverInterface
{
    public function __construct() {}

    /**
     * Observer for "nosto_sku_load_after" event
     *
     * @param Observer $observer
     * @return void
     */
    public function execute(Observer $observer)
    {
        /* @var $sku Sku */
        $sku = $observer->getSku();
        $data = $observer->getData();

        // This is for demonstration purposes only or to validate that
        // observer is working
        $sku->addCustomField('sku_customized','yes');

        // We must check that Magento\Catalog\Model\Product was passed correctly
        // in order to use attributes from the product
        if (isset($data['magentoProduct'])
            && $data['magentoProduct'] instanceof Product
        ) {
            /* @var Product $magentoProduct */
            $magentoProduct = $data['magentoProduct'];
            // Add attribute named myattribute into Nosto SKU
            if($magentoProduct->getCustomAttribute('myattribute')) {
                $sku->addCustomField('myattribute',
                    $magentoProduct->getCustomAttribute('myattribute')
                );
            }
        }
    }
}
```

##### 4. The module registration `app/code/My/Nosto/registration.php`

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'My_Nosto',
    __DIR__
);
```

##### 5. The composer `app/code/My/Nosto/composer.json`

```javascript
{
    "name": "my/nosto-extension",
    "description": "Custom extension for altering product data",
    "type": "magento2-module",
    "version": "1.0.0",
    "license": [
        "OSL-3.0"
    ],
    "minimum-stability": "dev",
    "require": {
        "php": ">=5.5.0",
        "nosto/magento2-extension": ">=0.1.0"
    },
    "autoload": {
        "psr-4": {
          "My\\Nosto\\": ""
        },
        "files": [ "registration.php" ]
    }
}
```

## Enabling the module

Once the modules files are in place you must enable the module. Depending on file permissions you might need to run these commands with sudo.

```bin/magento module:enable --clear-static-content My_Nosto```

```bin/magento setup:upgrade```

## Disabling the module

If you wish to disable the module there's no need to remove the module files. Simply run the following commands.

```bin/magento module:disable --clear-static-content My_Nosto```

```bin/magento setup:upgrade```

## More complex functionality needed in the observer?

If you need more complex functionality (like building URLs for example) you can use dependency injection in your observer. More about observers and DI can be found from Magento's [developer documentation](http://devdocs.magento.com/guides/v2.2/extension-dev-guide/events-and-observers.html)

## Using the module in production environment

Before using your custom module in production environment we strongly recommend that you follow the instructions how to package a Magento 2 module. If you are familiar with creating packages for composer, creating a package for M2 is a no-brainer. You can find the instructions from [here](http://devdocs.magento.com/guides/v2.0/extension-dev-guide/package/package_module.html)