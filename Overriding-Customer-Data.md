In order to modify the customer data that is sent to Nosto, you need to create your own mini-module. Please read the basics on [how to extend or override data in Nosto extension](Overriding-or-extending-functionalities)

For modifying Nosto's customer data you must register hook **`actionNostoCustomerLoadAfter`** and the **`actionNostoOrderLoadAfter`** and implement corresponding action methods. Below is an example of how your `MyNosto` class could look like.

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
        $this->author = 'YourName';
        $this->need_instance = 1;
        $this->bootstrap = true;
        $this->ps_versions_compliancy = array(
            'min' => '1.4',
            'max' => _PS_VERSION_
        );
        parent::__construct();
        $this->displayName = $this->l('My mini nosto plugin');
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
            && $this->registerHook('actionNostoCustomerLoadAfter')
            && $this->registerHook('actionNostoOrderLoadAfter')
        );
    }

    public function hookActionNostoCustomerLoadAfter(array $params)
    {
        /** @var NostoCustomer $nostoCustomer */
        $nostoCustomer = $params['nosto_customer'];

        //Set marketing permission for the customer, '1' or 'true' means the customer accepts newsletters
        $nostoCustomer->setMarketingPermission('1');
    }

    public function hookactionNostoOrderLoadAfter(array $params)
    {
        /** @var NostoOrder $nostoOrder */
        $nostoOrder = $params['nosto_order'];

        //Set marketing permission for the customer, '1' or 'true' means the customer accepts newsletters
        if ($nostoOrder->getCustomer() instanceof NostoOrderBuyer) {
            $nostoOrder->getCustomer()->setMarketingPermission('1');
        }

    }
}
```

## Verifying

Once you have overridden the customer model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to log in as a customer and view any page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://help.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.