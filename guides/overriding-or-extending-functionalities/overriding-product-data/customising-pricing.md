# Customising Pricing

If your store uses customized price handling or 3rd party module for product prices you might need to amend the custom price handling to Nosto module as well. Start by reading [how to extend Nosto module](../) and [how to alter Nosto product data](./).

**Note:** In order to use custom pricing in Nosto, you must amend the prices to Nosto's product model and handle the correct active variation. Please note that when using customized pricing you should disable the multi-currency method from the Nosto module's setting in Magento.

File `app/code/local/My/Nosto/Model/Meta/Product.php`

```php
<?php

/**
 * Overridden product meta data model.
 * This model is used as the data source when generating the tagging elements
 * on the product page, and when making server-to-server API calls to Nosto.
 */
class My_Nosto_Model_Meta_Product extends Nosto_Tagging_Model_Meta_Product
{
    /**
     * @inheritdoc
     */
    public function loadData(Mage_Catalog_Model_Product $product, Mage_Core_Model_Store $store = null)
    {
        parent::loadData($product, $store);

        $this->setPrice(111.11);
        $this->setListPrice(222.22);

        return true;
    }
}
```

