# Resizing Images

In order to use images with custom dimensions, you can use a built-in Magento re-sizer function to generate image URLs with the correct dimensions and you will need to amend the product model.

Start by reading [how to extend Nosto module](../) and [how to alter Nosto product data](./).

**Note:** In case you want a square image, only the first parameter is required.

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

        $height = 300; 
        $width = 200;

        $image = (string)Mage::helper('catalog/image')
            ->init($product, 'thumbnail')
            ->resize($height, $width);

        $this->setImageUrl($image);

        return true;
    }
}
```

