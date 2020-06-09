# Database caching

![](https://img.shields.io/badge/nosto-4.0.0%20%3C%205.0.0-green)

From version 4.0.0 Nosto uses a new table used to store and cache product data rather than build it on the fly. For version &gt;= 5.0.0 please refer [to this article](built-in-caching.md).

The cache contains the serialized Nosto product. As Nosto tags, the current product's metadata on the product pages, the cache ensures that the product is simply read from the cache and does not lead to redundant MySQL queries.

## Cache invalidation

A new cron job is added as a fallback mechanism for invalidating the cached product data. By default the cron runs every hours and invalidates cached product data that has been updated more than 4 hours ago. The default maximum amount of invalidated products in one run is 1000. Invalidated products are rebuild and compared against the current cached product data.

You can easily change the default look back interval and the processing limit by overriding the injected values defined in `di.xml`.

```markup
<type name="Nosto\Tagging\Cron\InvalidateCron">
    <arguments>
        <argument name="intervalHours" xsi:type="number">4</argument>
        <argument name="productLimit" xsi:type="number">1000</argument>
    </arguments>
</type>
```

If want to disable the invalidate cron you can set the `productLimit` to 0.

Remember that you must run `bin/magento setup:di:compile` for any changes done in the configuration files to take affect.

This cron job is removed in version  &gt;= 5.0.0 as the functionality is redundant when using Magento's built-in caching. 

