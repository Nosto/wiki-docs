# Customising Category data

If your store uses a customized logic for handling product's categories you might need to amend categories in Nosto module as well . Start by reading [how to extend Nosto module](../) and [how to alter Nosto product data](./).
 
If overriding the product data directly does not seem the best option for you, you can create a service class and make use of Dependency Injection pattern. 

Create a new class which will implement `CategoryServiceInterface`. Override the required functions and afterwards replace 
`CategoryServiceInterface` with `MyCategoryServive` in the DI `etc/di.xml`


File `app/code/My/Nosto/Model/Service/MyCategoryServive.php`

```php
<?php

namespace My\Nosto\Observer\Product;

use Nosto\Tagging\Model\Service\Product\Category\CategoryServiceInterface;
use Magento\Catalog\Model\Category;
use Magento\Catalog\Model\Product;
use Magento\Store\Api\Data\StoreInterface;

class MyCategoryServive implements CategoryServiceInterface
{   

    /**
     * Method used to return a single category string for given category object
     * @param Category $category
     * @param StoreInterface $store
     * @return null|string
     */
    public function getCategory(Category $category, StoreInterface $store) {
        //ToDo Implement logic    
    }   
    
    /**
     * Method used to return list of strings representing categories for a given product 
     * @param Product $product
     * @param StoreInterface $store
     * @return array
     */
    public function getCategories(Product $product, StoreInterface $store) {
    
    }   
}
```


