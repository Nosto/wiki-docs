# Overriding Product Data

In order to modify the product data that is sent to Nosto, you need to create your own plugin. Please read the basics on [how to extend or override data in Nosto extension](https://github.com/supercid/wiki-docs/tree/16e7063bba4e84d9e0e1b47564ffda8a7eb25486/Overriding-or-Extending-Functionalities.md)

For modifying Nosto's product data you must register hook **`onNostoTaggingComponentsModelProductAfterLoad`** and implement corresponding action method. Below is an example of how your `MyNosto` class could look like.

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
        /** @var Shopware_Plugins_Frontend_NostoTagging_Components_Model_Product $nostoProduct */
        $nostoProduct = $args->get('nostoProduct');
        /** @var \Shopware\Models\Article\Article $article */
        $article = $args->get('article');
        /** @var \Shopware\Models\Shop\Shop $shop */
        $shop = $args->get('shop');

        /*
         * Example 1
         *
         * Adding "tag1", "tag2" and "tag3" properties.
         *
         * These properties are used to include more information about the
         * product that can in turn be used for more granular control in the
         * product recommendations.
         *
         * The tags are string values that can be added to the product.
         */
        // $nostoProduct->addTag1('test_tag1');
        // $nostoProduct->addTag2('test_tag2');
        // $nostoProduct->addTag3('test_tag3');

        /* 
         * If you have additional data stored for the product as described in
         * http://community.shopware.com/Item-open-text-fields_detail_1115_831.html,
         * you can add them to the Nosto product meta-data like this:
         */
        // $attribute = Shopware()
        //     ->Models()
        //     ->getRepository('Shopware\Models\Attribute\Article')
        //     ->findOneBy(array('articleId' => $article->getId()));
        // if ($attribute) {
        //     /* 
        //      * Note the name of the getter method, it has to match your 
        //      * attribute name. You can add many attributes by duplicating 
        //      * the line below and changing the getter names accordingly.
        //      */
        //     $nostoProduct->addTag2($attribute->getMyAttribute());
        // }

        /* 
         * This can be removed when implementing this plugin, it's here only to
         * check that the override works when implemented.
         */
        $nostoProduct->addTag3('nosto');

        /*
         * Adding product price per reference units to the "tag2" property.
         *
         * This can be used to display the price per units in the product
         * recommendations, e.g. "Inhalt: 100 Gramm (1,199.50€ * / 1000 Gramm)".
         *
         * NOTE: this will not work with multi-currency, as the the price is
         * always tagged in the current displayed currency. For multi-currency
         * you have to tag the price per unit for all currencies and build the
         * display logic into the Nosto recommendation template.
         */
        // $mainDetail = $article->getMainDetail();
        // $unit = $mainDetail->getUnit();
        // // Check that we got both a unit and a price.
        // if ($unit && $nostoProduct->getPrice()) {
        //     // Get the unit, e.g. "g" for gram.
        //     $unitName = $unit->getUnit();
        //     $purchaseUnit = (double)$mainDetail->getPurchaseUnit();
        //     $referenceUnit = (double)$mainDetail->getReferenceUnit();
        //     // Use the price form the Nosto product, which already has both
        //     // discounts and taxes applied.
        //     $price = $nostoProduct->getPrice();
        //     // Convert the price into the current displayed currency.
        //     // The Nosto product price is always in the base currency.
        //     $price *= $shop->getCurrency()->getFactor();
        //     // Calculate the price per reference units.
        //     $referencePrice = $price / $purchaseUnit * $referenceUnit;
        //     // Format the price string according to Zend standards.
        //     $zendCurrency = new Zend_Currency(
        //         $shop->getCurrency()->getCurrency(),
        //         $shop->getLocale()->getLocale()
        //     );
        //     // Override the currency symbol position with the one configured for
        //     // the Shopware currency object.
        //     $zendCurrency->setFormat(array(
        //         'position' => ($shop->getCurrency()->getSymbolPosition() > 0
        //             ? $shop->getCurrency()->getSymbolPosition()
        //             : 8)
        //     ));
        //     $priceString = $zendCurrency->toCurrency($referencePrice);
        //
        //     // Add the tag to the Nosto product,
        //     // e.g. "Inhalt: 100 Gramm (1,199.50€ * / 1000 Gramm)".
        //     $nostoProduct->addTag2("Inhalt: {$purchaseUnit} {$unitName} ({$priceString} * / {$referenceUnit} {$unitName})");
        // }

        /*
         * Example 2
         *
         * Replacing all the tags for "tag1", "tag2" or "tag3" properties.
         *
         * The "tag2" and "tag3" properties will be empty by default, so this
         * method is an easy way to add a set of tag values without looping
         * through them. Be careful with the "tag1" property though, as that can
         * contain pre-defined values, which will be lost of using this method.
         * For the "tag1" property it is safer to use the method is `Example 1`.
         *
         * The format is an array containing string values.
         */
        // $nostoProduct->setTag1(array('test1_tag1', 'test2_tag1'));
        // $nostoProduct->setTag2(array('test1_tag2', 'test2_tag2'));
        // $nostoProduct->setTag3(array('test1_tag3', 'test2_tag3'));

        /*
         * Example 3
         *
         * Changing the product image URL.
         *
         * If you are, for example, using a 3rd party cloud storage for your
         * product images.
         *
         * The url must be absolute and includes the protocol (http or https).
         */
         // $nostoProduct->setImageUrl('http://example.com/p/example.jpg');
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

Once you have overridden the product model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to view any product page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

If you were to extend the product model using the example given above, you would see that the "Tags" field in the debug-toolbar will read "nosto".

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

