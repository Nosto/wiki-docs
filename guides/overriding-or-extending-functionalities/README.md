# Overriding or Extending Functionalities

You can extend the functionality of Nosto extension and modify what data is sent to Nosto.

In order to extend Nosto extension's functionality \(methods, data, etc.\) you must create a small Magento extension. This way, you can override the product model, order model or any other functionality provided by Nosto extension.

## Directory Structure

Creating an override requires a few files to be created. Follow the guide below and simply copy-paste the content into the specified locations. This will form the scaffold of your new override.

### 1. The module definition `app/etc/modules/My_Nosto.xml`

```markup
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

### 2. The module configuration `app/code/local/My/Nosto/etc/config.xml`

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
                    <meta_order>My_Nosto_Model_Meta_Order</meta_order>
                </rewrite>
            </nosto_tagging>
        </models>
    </global>
</config>
```

In the example configuration above we are overriding Nosto's Product and Order models. See full example how to override Product data [from here](overriding-product-data/)

## Enabling the plugin

The module does not need to be explicitly installed. Assuming that you have followed the instructions above, your module should be automatically enabled. You can verify this by running the following command.

```text
./mage list-installed
```

## Disabling the plugin

There is no way to disable a module without uninstalling it and removing all files. In order to remove it, you will need to either remove all the associated files manually or run the following command.

```text
./mage uninstall community MyNosto
```

## Verifying

Once you have overridden the associated model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to log in as a customer and view any page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

