In order to modify the product data that is sent to Nosto you must create a small Magento 2 module. Start by following the steps described in the wiki page [Extending Nosto's Module](Overriding-or-extending-functionalities). <br>
By creating a M2 module you can override the product model in a way that the modifications take place in API calls, exports and in the tagging.

## Required files and configurations

##### 1. The events config `app/code/My/Nosto/etc/events.xml`

```xml
<?xml version="1.0"?>
<!--suppress XmlUnboundNsPrefix -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Event/etc/events.xsd">
    <event name="nosto_product_load_after">
        <observer name="nosto_product_load_after" instance="My\Nosto\Observer\Product\Load" />
    </event>
</config>
```

##### 2. The observer `app/code/My/Nosto/Observer/Product/Load.php`

```php
<?php

namespace My\Nosto\Observer\Product;

use Magento\Framework\Event\Observer;
use Magento\Framework\Event\ObserverInterface;

class Load implements ObserverInterface
{
    public function __construct()
    {
    }

    /**
     * Observer for "nosto_product_load_after" event
     *
     * @param Observer $observer
     * @return void
     */
    public function execute(Observer $observer)
    {
        /* @var $product \NostoProduct */
        $product = $observer->getProduct();

        /*
        * Here you can modify any product property you need and it will
        * automatically be used both in the store front tagging and the
        * server-to-server API calls.
        *
        * Note that any new properties will not automatically be used.
        *
        * @see https://support.nosto.com/get-started/tagging-product-pages/
        */

        /*
         * Example 1
         *
         * Adding "tag2" and "tag3" properties.
         *
         * These properties are used to include more information about the
         * product that can in turn be used for more granular control in the
         * product recommendations.
         *
         * It is important to notice that the `addTag()` function receives a string as an argument
         * and can be called multiple times in order to add multiple arguments.
         *  If you call:
         *  $product->addTag3('example1');
         *  $product->addTag3('example2');
         *  You will have `example1` and `example2` as in the tag3 field.
         *
         * The `setTag` function takes an array of strings as argument and
         * will override any previously assigned values to the tag:
         *
         * $product->setTag2(['example1', 'example2']);
         * $product->setTag3(['example3']);
         * 
         */

        /*
         * Example 2
         *
         * Changing the product image URL.
         *
         * If you are, for example, using a 3rd party cloud storage for your
         * product images.
         *
         * The url must be absolute and includes the protocol (http or https).
         * $product->setImageUrl('http://example.com/p/example.jpg');
         *
         */

        // This can be removed when implementing this extension, it's here only to
        // check that the override works when implemented.
        $product->addTag3('nosto');
    }
}
```

#### Adding custom attributes to product tags
```php
<?php

namespace My\Nosto\Observer\Product;

use Magento\Framework\Event\Observer;
use Magento\Framework\Event\ObserverInterface;
use Magento\Catalog\Api\ProductRepositoryInterface;

class Load implements ObserverInterface
{
    protected $productRepository;

    public function __construct(ProductRepositoryInterface $productRepository)
    {
        $this->productRepository = $productRepository;
    }

    /**
     * Observer for "nosto_product_load_after" event
     *
     * @param Observer $observer
     * @return void
     * @throws \Magento\Framework\Exception\NoSuchEntityException
     */
    public function execute(Observer $observer)
    {
        /* @var $product \NostoProduct */
        $product = $observer->getProduct();
        // Get the Magento product
        $magentoProduct = $this->productRepository->getById($product->getProductId());

        // If you have a custom attribute (string value) stored in the Magento
        // product model, it can be added to a tag field like this:
        $myAttribute = $magentoProduct->getCustomAttribute('my_custom_attribute');
        if($myAttribute) {
            $product->addTag3($myAttribute->getValue());
        }
    }
}


```
If all the steps were followed correctly, your final directory structure should have these files:
```bash
app
├── code
│   └── My
│       └── Nosto
│           ├── Observer
│           │   └── Product
│           │       └── Load.php
│           ├── composer.json
│           ├── etc
│           │   ├── events.xml
│           │   └── module.xml
│           ├── registration.php
```
Run the following commands to update the generated code in Magento's end <br>
`bin/magento module:enable --clear-static-content My_Nosto`

`bin/magento setup:upgrade`

As of module version 4.0.0 [Nosto product data is cached in database](https://github.com/Nosto/nosto-magento2/wiki/Caching-Improvements) and for this you must run full reindex (`bin/magento indexer:reindex nosto_index_product_invalidate`) in order for the changes to be reflected to Nosto.