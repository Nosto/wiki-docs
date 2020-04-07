# Overriding Product Data

In order to modify the product data that is sent to Nosto, you need to create your own mini-module. Please read the basics on [how to extend or override data in Nosto extension](./)

For modifying Nosto's product data you must register hook **`actionNostoProductLoadAfter`** and implement corresponding action method. Below is an example of how your `MyNosto` class could look like.

File `modules/mynosto/mynosto.php`

```php
<?php

if (!defined('_PS_VERSION_')) exit;

/**
 * Module for overriding Nosto default behavior.
 *
 * Depends on the "Personalization for PrestaShop" module by Nosto.
 */
class MyNosto extends Module

{
    /**
     * Constructor.
     *
     * Defines the module.
     */
    public function __construct()
    {
        $this->name = 'mynosto';
        $this->tab = 'advertising_marketing';
        $this->version = '0.1.0';
        $this->author = 'Nosto';
        $this->need_instance = 1;
        $this->bootstrap = true;
        $this->ps_versions_compliancy = array(
            'min' => '1.4',
            'max' => _PS_VERSION_
        );
        parent::__construct();
        $this->displayName = $this->l('My Nosto');
        $this->description = $this->l('Module for overriding Nosto default behavior');
    }

    /**
     * Module installer.
     *
     * Registers the event listener for nosto events
     *
     * @return bool
     */
    public function install()
    {
        return (
            parent::install()
            && $this->registerHook('actionNostoProductLoadAfter')
        );
    }

    /**
     * Action method for modifying the Nosto product information before it is used the shop frontend or the server-to-server API.
     *
     * The input params will always include:
     *
     * - "nosto_product" which is the NostoTaggingProduct instance that has just been loaded with data
     * - "product" which is the Prestashop Product model instance used as source for the data
     * - "context" which is the Prestashop Context model instance used when loading the data
     *
     * Modifying the instance in $params['nosto_product'] will directly reflect on the data that is sent to Nosto. The
     * NostoTaggingProduct class has a public API of setter methods to modify it's content. Some common use cases are
     * document in below method.
     *
     * You can find a full reference of the available product fields in the Nosto support center.
     *
     * @param array $params
     *
     * @see https://support.nosto.com/get-started/tagging-product-pages/
     */
    public function hookActionNostoProductLoadAfter(array $params)
    {
        /** @var NostoProduct $nosto_product */
        $nosto_product = $params['nosto_product'];

        /** @var NostoTaggingProduct $prestashop_product_product */
        $prestashop_product = $params['product'];

        if ($nosto_product instanceof NostoProduct) {
           // Implement your customizations here
          $nosto_product->addTag3('nosto test');
          $nosto_product->setPrice($nosto_product->getPrice()*1.2);
          $nosto_product->setName($nosto_product->getName() . ' - TEST OVERRIDE');
        }
    }
}
```

## Verifying

Once you have overridden the product model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to view any product page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://help.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

If you were to extend the product model using the example given above, you would see that the "Tags" field in the debug-toolbar will read "nosto".

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

