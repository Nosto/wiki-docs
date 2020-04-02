You can extend the functionality of Nosto module and modify what data is sent to Nosto.

In order to extend Nosto module, you must create a small Shopware plugin. This way, you can override the data in product model, order model or any other functionality provided by Nosto module. All Nosto objects - including the product, customer, order, category - when built, dispatch events that can be hooked into and overridden. We recommend reading up on [Shopware's Event System](https://developers.shopware.com/developers-guide/event-guide/).

![2.1.0](https://img.shields.io/badge/nosto-2.1.0-green.svg)

The plugin requires a single file to work. Below you can find an example plugin which you can simply copy and paste into your Shopware plugins folder inside your installation. Let's call the module `My Nosto`. First, create a folder, `MyNosto`, inside your Shopware plugins folder, i.e. `SHOPWARE/engine/Shopware/Plugins/Local/Frontend/MyNosto`. Then copy paste the file below into that new folder.

**NOTE:** if you cannot make changes directly inside your Shopware installation, you can create a `Frontend/MyNosto` folder structure locally on your own computer and put the file inside the `MyNosto` folder, after which you create a zip archive of the `Frontend` folder that you can then upload in your Shopware backend like any other plugin.

File `Bootstrap.php`

```php
<?php

/**
 * Plugin for overriding Nosto default behavior.
 *
 * Depends on the "Personalization for Shopware" plugin by Nosto.
 * Extends Shopware_Components_Plugin_Bootstrap.
 *
 * @package Shopware
 * @subpackage Plugins_Frontend
 */
class Shopware_Plugins_Frontend_MyNosto_Bootstrap extends Shopware_Components_Plugin_Bootstrap
{
    /**
     * @inheritdoc
     */
    public function getCapabilities()
    {
        return array(
            'install' => true,
            'update' => true,
            'enable' => true
        );
    }

    /**
     * @inheritdoc
     */
    public function getLabel()
    {
        return 'My Nosto';
    }

    /**
     * @inheritdoc
     */
    public function getVersion()
    {
        return '0.1.0';
    }

    /**
     * @inheritdoc
     */
    public function getInfo()
    {
        return array(
            'version' => $this->getVersion(),
            'label' => $this->getLabel(),
            'source' => $this->getSource(),
            'author' => 'Unknown',
            'supplier' => 'Unknown',
            'copyright' => 'Copyright (c) 2015, Unknown',
            'description' => 'Plugin for overriding Nosto default behavior',
            'support' => 'support@example.com',
            'link' => 'http://example.com'
        );
    }

    /**
     * @inheritdoc
     */
    public function install()
    {
        $this->registerMyEvents();
        return true;
    }

    /**
     * @inheritdoc
     */
    public function uninstall()
    {
        return true;
    }

    /**
     * @inheritdoc
     */
    public function update($version)
    {
        return true;
    }

    /**
     * Event listener for `Shopware_Plugins_Frontend_NostoTagging_Components_Model_Product_AfterLoad`.
     *
     * Listener for modifying the Nosto product information before it is used in
     * the shop frontend or the server-to-server API.
     *
     * The event arguments will always include:
     *
     * - "nostoProduct" the Shopware_Plugins_Frontend_NostoTagging_Components_Model_Product
     * - "article" the Shopware Article
     * - "shop" the Shopware Shop
     *
     * Modifying the product object will directly reflect on the data that is
     * sent to Nosto. The object has a public API of setter methods to modify
     * it's content. Some common use cases are document in below method.
     *
     * You can find a full reference of the available product fields in the
     * Nosto support center.
     *
     * @param Enlight_Event_EventArgs $args
     *
     * @see https://support.nosto.com/get-started/tagging-product-pages/
     */
    public function onNostoTaggingComponentsModelProductAfterLoad(Enlight_Event_EventArgs $args)
    {
        // Do whatever
    }

    /**
     * Registers events for this plugin.
     *
     * Run on install.
     */
    protected function registerMyEvents()
    {
        /*
         * Register an event listener for the Nosto product object "AfterLoad"
         * event. This event is dispatched when the product object has been
         * loaded with data, before it is used in either the frontend or in the
         * server-to-server API calls. At this point you can modify it's content
         * that is sent to Nosto.
         */
        $this->subscribeEvent(
            'Shopware_Plugins_Frontend_NostoTagging_Components_Model_Product_AfterLoad',
            'onNostoTaggingComponentsModelProductAfterLoad'
        );
    }
}
```

Once you've gotten the plugin into your Shopware installation, you should see it inside your Shopware Plugin Manager. Click the `active` button next to the plugin to activate it and clear the shop cache. Now you can start modifying the product information. The changes you make will be taken into use instantly both in the shop.

How does it work? The Nosto plugin will dispatch an event called `Shopware_Plugins_Frontend_NostoTagging_Components_Model_Product_AfterLoad` whenever the product is loaded for frontend or backend tasks. By listening to this event you can override the object before it is used, and thus your changes will be included when the data is sent to Nosto.

## Verifying

Once you have overridden the associated model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to log in as a customer and view any page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

## Hooks dispatched by Nosto module

Nosto module dispatches following hooks.