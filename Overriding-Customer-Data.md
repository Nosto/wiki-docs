In order to modify the customer data that is sent to Nosto, you need to create your own extension. Please read the basics on [how to extend or override data in Nosto extension](Overriding-or-extending-functionalities)

## Verifying

Once you have overridden the product model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to view any product page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

If you were to extend the product model using the example given above, you would see that the "Tags" field in the debug-toolbar will read "nosto".

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

## Required files and configurations

##### 1. The module configuration `app/code/local/My/Nosto/etc/config.xml`

```xml
<?xml version="1.0"?>
<config>
    <modules>
        <My_Nosto>
            <active>true</active>
            <codePool>local</codePool>
            <depends>
                <Nosto_Tagging/>
            </depends>
        </My_Nosto>
    </modules>
</config>
```



##### 2. The events config `app/code/My/Nosto/etc/config.xml`

```xml
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
                    <meta_order_buyer>My_Nosto_Model_Meta_Order_Buyer</meta_order_buyer>
                </rewrite>
            </nosto_tagging>
        </models>
        <blocks>
            <nosto_tagging>
                <rewrite>
                    <customer>My_Nosto_Block_Customer</customer>
                </rewrite>
            </nosto_tagging>
        </blocks>
    </global>
</config>
```

##### 3. The customer block `app/code/local/My/Nosto/Block/Customer.php`

```php
<?php

    /**
     * Overridden customer block data.
     * This model is used as the data source when generating the tagging elements
     * on the customer page, and when making server-to-server API calls to Nosto.
     */
    class My_Nosto_Block_Customer extends Nosto_Tagging_Block_Customer
    {
        /**
         * @inheritdoc
         */
        public function getNostoCustomer()
        {
            $nostoCustomer = parent::getNostoCustomer();

            if ($nostoCustomer->getEmail()) {
                // Do your extra validation here
            }

            return $nostoCustomer;
        }
    }

```
##### 4. The buyer model `app/code/local/My/Nosto/Model/Meta/Order/Buyer.php`
```php
<?php

    /**
     * Overridden buyer meta data model.
     * This model is used as the data source when
     * making server-to-server API calls to Nosto.
     */
    class My_Nosto_Model_Meta_Order_Buyer extends Nosto_Tagging_Model_Meta_Order_Buyer
    {
        /**
         * @inheritdoc
         */
        public function loadData(Mage_Sales_Model_Order $order)
        {
            parent::loadData($order);

            if ($this->getEmail()) {
                // Do your extra validation here
            }

            return true;
        }
    }
```