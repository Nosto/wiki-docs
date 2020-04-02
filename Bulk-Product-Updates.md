Bulk product updates can be done by building a custom Magento shell command. Below is an example of a Magento shell command. The following command loads the most recently 100 updated products and bulk updates them to Nosto.

Please note that in order to use the example implementation below you must have at least 2.10.0 of the Nosto extension installed in your store.

Once placed into the `shell` directory of your Magento installation, the following command invokes the process.
```
php -f nostoProductUpdate.php
```

We do not recommend updating products in batches of more than 100 at a time. If you exceed this limit, you might hit the hard limits of maximum POST request size of 2MB.

```php
<?php
/**
 * Magento
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Open Software License (OSL 3.0)
 * that is bundled with this package in the file LICENSE.txt.
 * It is also available through the world-wide-web at this URL:
 * http://opensource.org/licenses/osl-3.0.php
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@magentocommerce.com so we can send you a copy immediately.
 *
 * DISCLAIMER
 *
 * Do not edit or add to this file if you wish to upgrade Magento to newer
 * versions in the future. If you wish to customize Magento for your
 * needs please refer to http://www.magentocommerce.com for more information.
 *
 * @category  Nosto
 * @package   Nosto_Tagging
 * @author    Nosto Solutions Ltd <magento@nosto.com>
 * @copyright Copyright (c) 2013-2017 Nosto Solutions Ltd (http://www.nosto.com)
 * @license   http://opensource.org/licenses/osl-3.0.php  Open Software License (OSL 3.0)
 */
require_once 'abstract.php';

class Mage_Shell_Nosto_Product_Update extends Mage_Shell_Abstract
{
    const UPDATED_AT = 'updated_at';
    const CREATED_AT = 'created_at';
    public static $intervalInHrs = 1;
    /**
     * Get products to be updated
     * IMPORTANT! Implement here the logic to only fetch the updated products
     *
     * @return array
     */
    protected function getProducts($start = 1, $limit = 100, DateTimeInterface $updatedAt)
    {
        $products = Mage::getModel('nosto_tagging/product')->getCollection();
        $products->addAttributeToSelect('*')
            ->addFieldToFilter(
                self::UPDATED_AT, array(
                    'gteq' => $updatedAt->format('Y-m-d H:i:s')
                )
            )
            ->setPageSize($limit)
            ->setCurPage($start)
            ->setOrder(
                self::CREATED_AT,
                Varien_Data_Collection::SORT_ORDER_DESC
            );
        return $products;
    }
    /**
     * Run script
     *
     */
    public function run()
    {
        $date = new DateTime('now');
        $intervalDefinition = sprintf('PT%dH', self::$intervalInHrs);
        $interval = new DateInterval($intervalDefinition);
        $date->sub($interval);
        $products = $this->getProducts(1, 5, $date);
        $productCount = count($products);
        if ($productCount > 0) {
            printf('Updating %d to Nosto', $productCount);
            $service = new Nosto_Tagging_Model_Service_Product();
            $service->updateBatch($products);
            printf("\n" . 'All done' . "\n");
        } else {
            printf('Nothing to update');
        }
    }
    /**
     * Retrieve Usage Help Message
     *
     */
    public function usageHelp()
    {
        return <<<USAGE
Usage:  php -f nostoProductUpdate.php
  help              This help
USAGE;
    }
}

$shell = new Mage_Shell_Nosto_Product_Update();
$shell->run();
```

## FAQ

**Do I need to send my entire catalog with every cron update?**

No, if possible, only send the products that have changed.

**How many products can I send in one update?**

You may send as many products as you want so long as the entire payload is under 2MB. We recommend starting off with  a batch sizes of 50 and tweaking as you go along.

**How often should I run the cron?**

Crons should be run as often as you synchronise catalog data. Failure to do so may result in incorrect or invalid products being recommended.

**How should discontinued products be handled?**

In order to flag products as discontinued, you will need to deduce what those products are and sync those. Any product that out-of-stock, discontinued, invisible or unavailable (for whatever reason), will not be recommended by Nosto.