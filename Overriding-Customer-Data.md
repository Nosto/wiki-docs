In order to modify the customer data that is sent to Nosto, you need to create your own plugin. Please read the basics on [how to extend or override data in Nosto extension](Overriding-or-extending-functionalities)

For modifying Nosto's customer data you must register hook **`onNostoTaggingComponentsModelCustomerAfterLoad`** and implement corresponding action method. Below is an example of how your `MyNosto` class could look like.

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
            'copyright' => 'Copyright (c) 2018, Unknown',
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
     * Event listener for `Shopware_Plugins_Frontend_NostoTagging_Components_Model_Customer_AfterLoad`.
     *
     * Listener for modifying the Nosto customer information before it is used in
     * the shop frontend or the server-to-server API.
     *
     * The event arguments will always include:
     *
     * - "nostoCustomer" the Shopware_Plugins_Frontend_NostoTagging_Components_Model_Customer
     *
     * Modifying the customer object will directly reflect on the data that is
     * sent to Nosto. The object has a public API of setter methods to modify
     * it's content.
     *
     * @param Enlight_Event_EventArgs $args
     *
     * @return bool
     */
    public function onNostoTaggingComponentsModelCustomerAfterLoad(Enlight_Event_EventArgs $args)
    {
        /** @var Shopware_Plugins_Frontend_NostoTagging_Components_Model_Customer $nostoCustomer */
        $nostoCustomer = $args->get('nostoCustomer');

        if ($nostoCustomer->getEmail()) {
            // Do your extra validation here
            // $nostoCustomer->setMarketingPermission(true);
        }

        return $nostoCustomer->getMarketingPermission();
    }

    /**
     * Registers events for this plugin.
     *
     * Run on install.
     */
    protected function registerMyEvents()
    {
        /*
         * Register an event listener for the Nosto Customer object "AfterLoad"
         * event. This event is dispatched when the customer object has been
         * loaded with data, before it is used in either the frontend or in the
         * server-to-server API calls. At this point you can modify it's content
         * that is sent to Nosto.
         */
        $this->subscribeEvent(
            'Shopware_Plugins_Frontend_NostoTagging_Components_Model_Customer_AfterLoad',
            'onNostoTaggingComponentsModelCustomerAfterLoad'
        );
    }
}
```

Once you've gotten the plugin into your Shopware installation, you should see it inside your Shopware Plugin Manager. Click the `active` button next to the plugin to activate it and clear the shop cache. Now you can start modifying the customer information. The changes you make will be taken into use instantly both in the shop.

How does it work? The Nosto plugin will dispatch an event called `Shopware_Plugins_Frontend_NostoTagging_Components_Model_Customer_AfterLoad` whenever the customer is loaded for frontend or backend tasks. By listening to this event you can override the object before it is used, and thus your changes will be included when the data is sent to Nosto.

## Verifying

Once you have overridden the customer model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to log in as a customer and view any page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.