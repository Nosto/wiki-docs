In order to exclude certain products from being indexed, you will need to override the Nosto product model and flag it to be excluded. 

Start by reading [how to extend Nosto module](Overriding-or-extending-functionalities.md) and [how to alter Nosto product data](Overriding-Product-Data.md).

File `app/code/My/Nosto/Observer/Product/Load.php`

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
     * Observer for "nosto_product_load_before" event
     *
     * @param Observer $observer
     * @return void
     */
    public function execute(Observer $observer)
    {
        /* @var $product \NostoProduct */
        $product = $observer->getProduct();
        $modelFilter = $observer->getData('modelFilter');

        // If the product meets your desired criteria, set valid to false
        foreach ($product->getCategories() as $category) {
            if ($category === 'category') {
                $modelFilter->setValid(false);
            }
        }
    }
}
```