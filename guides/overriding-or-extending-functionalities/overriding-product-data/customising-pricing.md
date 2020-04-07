# Customising Pricing

If your store uses customized price handling or 3rd party module for product prices you might need to amend the custom price handling to Nosto module as well. Start by reading [how to extend Nosto module](../) and [how to alter Nosto product data](./).

**Note:** In order to use custom pricing in Nosto, you must amend the prices to Nosto's product model and handle the correct active variation. Please note that when using customized pricing you should disable the multi-currency method from the Nosto module's setting in Magento.

File `app/code/My/Nosto/Observer/Product/Load.php`

```php
<?php

namespace My\Nosto\Observer\Product;

use Magento\Catalog\Model\Product;
use Magento\Framework\Event\Observer;
use Magento\Framework\Event\ObserverInterface;
use Nosto\Tagging\Model\ElementFilter;

class Load implements ObserverInterface
{
    /**
     * IMPORTANT! You might need to inject additional services here in order to
     * get the custom prices for the product
     */
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
        /* @var $product \Nosto\Object\Product\Product */
        $nostoProduct = $observer->getProduct();

        /* @var $product \Magento\Catalog\Model\Product */
        $observerData = $observer->getData();
        // Check that Magento product is set and get the custom prices from Magento product model
        if (!empty($observerData['magentoProduct']) && $observerData['magentoProduct'] instanceof Product) {
            $magentoProduct = $observerData['magentoProduct'];
            $nostoProduct->setPrice($magentoProduct->getCustomFinalPrice());
            $nostoProduct->setListPrice($magentoProduct->getCustomPrice());
        }
    }
}
```

