# Customizing exclude filters

If custom product filtering is done on Magento's backend after receiving the GraphQL response from Nosto, this will cause the total counter of displayed products to be inaccurate.
To fix this, we recommend adding a custom field to the products and use exclude filters to be sent along with the GraphQL query

### Adding exclude filters to GraphQL query

1. Make sure you followed our guide to create an override module [Overriding or Extending Functionalities](addons/cmp/guides/overriding-or-extending-functionalities/README.md)
2. Use the sample code snippet below to add your custom exclusion logic

```php
<?php

namespace MyNosto\NostoCmp\Plugin;

use Nosto\Cmp\Model\Facet\Facet;

class UpdateExcludedFilters
{
    /**
     * @param $subject
     * @param Facet $facets
     * @return Facet
     */
    public function afterGetFilters($subject, Facet $facets)
    {
        $facets->getExcludeFilters()->setCustomFields(
            "my_custom_field",
            ['my_custom_field_value']
        );

        return new Facet($facets->getIncludeFilters(), $facets->getExcludeFilters());
    }
}
```