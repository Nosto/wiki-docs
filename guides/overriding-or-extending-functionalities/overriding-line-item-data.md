# Overriding Line Item Data

By default you do not need to make changes to the line item handling. Nosto's extension handles orders and line items correctly with the default checkout process. However if you need to for example alter the product data for simple products you can do it by overriding class `Nosto_Tagging_Model_Meta_Order_Item_Simple`\).

Please [read here the basics how to alter the functionalities of Nosto extension](./).

## Required files and configurations

### 1. The module configuration `app/code/local/My/Nosto/etc/config.xml`

```markup
<?xml version="1.0"?>
<!-- app/code/local/My/Nosto/etc/config.xml -->
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
                    <meta_order_item_simple>My_Nosto_Model_Item</meta_order_item_simple>
                </rewrite>
            </nosto_tagging>
        </models>
    </global>
</config>
```

### 2. The overridden line item model `app/code/local/My/Nosto/Model/Item.php`

```php
<?php
// app/code/local/My/Nosto/Model/Item.php
/**
 * Overridden order item model.
 * IMPORTANT! This cannot be used as is. You need to figure out the correct way
 * to use this class.
 *
 * This class is for demonstration purposes only!
 *
 */
class My_Nosto_Model_Item extends Nosto_Tagging_Model_Meta_Order_Item_Simple
{

    /**
     * Customized load data
     * @return boolean
     */
    public function loadData(Mage_Sales_Model_Order_Item $item, $currencyCode)
    {
        parent::loadData($item, $currencyCode);
        $this->setName('CUSTOMIZED - ' . $this->getName());
    }
}
```

## Verifying

Once you have overridden the product model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to view any product page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

If you were to extend the product model using the example given above, you would see that the "Tags" field in the debug-toolbar will read "nosto".

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

