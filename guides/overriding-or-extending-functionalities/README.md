# Overriding or Extending Functionalities

You can extend the functionality of Nosto module and modify what data is sent to Nosto.

In order to extend Nosto module, you must create a small Prestashop module. This way, you can override the data in product model, order model or any other functionality provided by Nosto module.

![2.5.0](https://img.shields.io/badge/nosto-2.5.0-green.svg)

You need two configuration files for your custom module - the main module file and the actual module configuration that defines the overrides.

Below you can find an example module which you can simply copy&paste into your PrestaShop `modules` folder inside your installation. Let's call the module `My Nosto`. First create a folder, `mynosto`, inside your PrestaShop `modules` folder. Then copy paste these 2 files below into that new folder.

**Note:** if you cannot make changes directly inside your PrestaShop installation, you can create the `mynosto` folder locally on your own computer and put the files inside it, after which you create a zip archive of the whole folder that you can then upload in your PrestaShop backend like any other module.

## Directory Structure

Creating an override requires a few files to be created. Follow the guide below and simply copy-paste the content into the specified locations. This will form the scaffold of your new override.

### 1. The module configuration `modules/mynosto/config.xml`

```markup
<?xml version="1.0" encoding="UTF-8" ?>
<module>
    <name>mynosto</name>
    <displayName><![CDATA[My Nosto]]></displayName>
    <version><![CDATA[0.1.0]]></version>
    <description><![CDATA[Module for overriding Nosto default behavior]]></description>
    <author><![CDATA[Nosto]]></author>
    <tab><![CDATA[advertising_marketing]]></tab>
    <is_configurable>0</is_configurable>
    <need_instance>1</need_instance>
    <limited_countries></limited_countries>
</module>
```

### 2. The module class `modules/mynosto/mynosto.php`

This file contains the actual override and follows the conventions of creating a standard Prestashop module. It is in this class that you will need to register the subscribers or hooks for whatever event you will be listening for.

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
     * Registers the hooks you want to observe
     *
     * Note that the hook must have corresponding action method. For example actionExampleLoadAfter must have
     * method hookActionExampleLoadAfter which is called when hook actionExampleLoadAfter is fired
     * @return bool
     */
    public function install()
    {
        return (parent::install()
            && $this->registerHook('actionExampleLoadAfter')
        );
    }

    /**
     * Action method for hook actionExampleLoadAfter
     *
     * @param array $params
     */
    public function hookActionExampleLoadAfter(array $params)
    {
        if (!empty($params['nosto_object']) && is_object($params['nosto_object'])) {
            $nosto_object = $params['nosto_object'];
            // Add your customizations here
        }
    }
}
```

## Enabling the plugin

Once you've got the module into your PrestaShop installation, you should see it inside your PrestaShop backend module listing. Click the `Install` button next to the module to install it. Now you can start modifying the product information. The changes you make will be taken into use instantly both in the shop frontend \(you can view the changes in the handy Nosto debug toolbar when navigating the product pages\) and in the backend API requests.

If you are using PrestaShop 1.7 you should install the plugin through the command line using the command `bin/console prestashop:module install mynosto`.

Please note that if you add or remove registered hooks you need to Reset the custom module in PrestaShop backend module listing.

## Verifying

Once you have overridden the associated model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to log in as a customer and view any page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://help.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

## Hooks dispatched by Nosto module

Nosto module dispatches following hooks.

| Object | Hook |
| :--- | :--- |
| `NostoBrand` | `actionNostoBrandLoadAfter` |
| `NostoCart` | `actionNostoCartLoadAfter` |
| `NostoCategory` | `actionNostoCategoryLoadAfter` |
| `NostoCurrentUser` | `actionNostoCurrentUserLoadAfter` |
| `NostoCurrentVariation` | `actionNostoCurrentVariationLoadAfter` |
| `NostoCustomer` | `actionNostoCustomerLoadAfter` |
| `NostoExchangeRates` | `actionNostoExchangeRatesLoadAfter` |
| `NostoIframe` | `actionNostoIframeLoadAfter` |
| `NostoOauth` | `actionNostoOAuthLoadAfter` |
| `NostoOrder` | `actionNostoOrderLoadAfter` |
| `NostoProduct` | `actionNostoProductLoadAfter` |
| `NostoSearch` | `actionNostoBrandLoadAfter` |
| `NostoAccountBilling` | `actionNostoAccountBillingLoadAfter` |
| `NostoAccountOwner` | `actionNostoAccountOwnerLoadAfter` |
| `NostoAccountSignup` | `actionNostoAccountSignupLoadAfter` |
| `NostoOrderBuyer` | `actionNostoOrderBuyerLoadAfter` |
| `NostoOrderStatus` | `actionNostoOrderStatusLoadAfter` |
| `NostoVariationKeyCollection` | `actionVariationKeyCollectionLoadAfter` |

