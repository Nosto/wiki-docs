# Using custom value for category page size

The CM module makes use of the default page size used in the storefront. There might be cases when the page size value might be incorrect. You can state the page size used by our module.

### Use cases

There are 3 use cases:

1. When MySQL is configured as the search engine.
2. When ES is configured as the search engine
3. When the merchant is using M2 graphql to fetch the catalog page

### Setting custom page size value

1. Open the file `app/code/My/Nosto/etc/di.xml`
2. Override the pageSize argument for web handler `Nosto\Cmp\Plugin\Framework\Search\Request\WebHandler` or GraphQL handler `Nosto\Cmp\Plugin\Framework\Search\Request\GraphQlHandler`

```markup
    <type name="Nosto\Cmp\Plugin\Framework\Search\Request\WebHandler">
        <arguments>
            <argument name="pageSize" xsi:type="number">64</argument>
        </arguments>
    </type>
    <type name="Nosto\Cmp\Plugin\Framework\Search\Request\GraphQlHandler">
        <arguments>
            <argument name="pageSize" xsi:type="number">64</argument>
        </arguments>
    </type>
```

### Compiling your changes 

In order to compile your changes you'll need to run the `di:compile` command

```markup
bin/magento setup:di:compile
```