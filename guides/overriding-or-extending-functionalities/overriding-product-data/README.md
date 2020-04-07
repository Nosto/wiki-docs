# Overriding Product Data

You can override the product model Nosto extension uses as data source for both the tagging in the store front and the API calls. Please [read here the basics how to alter the functionalities of Nosto extension](../).

**Note:** As of extension version &gt; 3.0.0 you must use getters and setters to populate and access Nosto product object attributes.

## Required files and configurations

### 1. The module configuration `app/code/local/My/Nosto/etc/config.xml`

```markup
<?xml version="1.0"?>
<config>
    <modules>
        <My_Nosto>
            <version>0.1.0</version>
        </My_Nosto>
    </modules>
    <global>
        <models>
            <nosto_tagging>
                <rewrite>
                    <meta_product>My_Nosto_Model_Meta_Product</meta_product>
                </rewrite>
            </nosto_tagging>
        </models>
    </global>
</config>
```

### 2. The overridden product model `app/code/local/My/Nosto/Model/Meta/Product.php`

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
         * If you have a custom attribute (string value) stored in the Magento
         * product model, it can be added to a tag field like this:
         *
         * $this->addTag2($product->getData('my_custom_attribute'));
         *
         * Or by fetching the attribute text:
         *
         * $this->addTag2($product->getAttributeText('my_custom_attribute'));
         *
         * If you have values stored in a custom model that are linked to the
         * product, you could do something like this:
         *
         * $this->addTag2(Mage::getModel('my_extension/model')->getProductAttributes($product));
         * 
         * It is important to notice that the `addTag()` function receives a string as an argument
         * and can be called multiple times in order to add multiple arguments.
         *  If you call:
         *  $this->addTag3('example1');
         *  $this->addTag3('example2');
         *  You will have `example1` and `example2` as in the tag3 field.
         *
         * The `setTag` function takes an array of strings as argument and 
         * will override any previously assigned values to the tag:
         *
         * $this->setTag2(['example1', 'example2']);
         * $this->setTag3(['example3']);
         */


        // This can be removed when implementing this extension, it's here only to
        // check that the override works when implemented.
        $this->addTag3('nosto');

        /*
         * Example 2
         *
         * Changing the product image URL.
         *
         * If you are, for example, using a 3rd party cloud storage for your
         * product images.
         *
         * The url must be absolute and includes the protocol (http or https).
         */
//        $this->setImageUrl('http://example.com/p/example.jpg');

         return true;
    }
}
```

After this, you can modify the `Product.php` file according to your needs. Clearing the Magento cache will make the extension work.

## Verifying

Once you have overridden the product model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to view any product page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

If you were to extend the product model using the example given above, you would see that the "Tags" field in the debug-toolbar will read "nosto".

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

