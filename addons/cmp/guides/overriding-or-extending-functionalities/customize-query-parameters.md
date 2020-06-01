# Customize Query Parameters

This module relies on Http parameters for handling pagination and sorting. In case your Magento installation has a customised catalog page or customised query parameter \(we rely on `p` for page and `product_list_order` for sorting order\), it is necessary to create a class which will be responsible for handling two parameters: page number and sorting order.  Read up on [Overriding or Extending Functionalities](./) prior to proceeding with this.

### Create a `CustomParameterResolver` class

This class will implement `ParameterResolverInterface` and it's methods.

```php
<?php

namespace My\Nosto\Plugin;

use Nosto\Cmp\Plugin\Catalog\Block\ParameterResolverInterface;

class CustomParameterResolver implements ParameterResolverInterface
{
    /**
     * @inheritdoc
     */
    public function getSortingOrder()
    {
        //ToDo return current sorting order
    }

    /**
     * @inheritdoc
     */
    public function getCurrentPage()
    {
        //ToDo return current page number
    }
}
```

### Inject `CustomParameterResolver`

1. Open the file `app/code/My/Nosto/etc/di.xml`
2. Inject the class

```markup
   <type name="Nosto\Cmp\Plugin\Catalog\Block\Pager">
        <arguments>
            <argument name="parameterResolver" xsi:type="object">
                My\Nosto\Plugin\CustomParameterResolver
            </argument>
        </arguments>
    </type>
    <type name="Nosto\Cmp\Plugin\Catalog\Block\Toolbar">
        <arguments>
            <argument name="parameterResolver" xsi:type="object">
                My\Nosto\Plugin\CustomParameterResolver
            </argument>
        </arguments>
    </type>
```

### Compiling your changes 

In order to compile your changes you'll need to run the `di:compile` command

```markup
bin/magento setup:di:compile
```

