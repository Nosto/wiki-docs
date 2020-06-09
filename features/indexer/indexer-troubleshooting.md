# Indexer troubleshooting

If you are having issues with indexing you want to first enable Magento's debug logging [https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html](https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html). This will enable more verbose logging for the indexing. You will find indexing related logs from debug log \(`debug.log` by default\). All log entries are prefixed with "nosto".

### Indexer is not keeping up with product updates

If you are frequently updating massive amount of products \(for example via API or import\) there's a chance that the indexer cannot process the previous update before the next update batch is executed. In these cases we recommend [parallelising the indexer](./#indexer-parallelisation) as a first step.

We also recommend figuring out the source of frequent product updates and do optimisations for the [mview subscriptions / triggers](https://github.com/Nosto/nosto-magento2/blob/master/etc/mview.xml#L38). For example if you are using 3rd party module / integration that updates all product images frequently but those images are not used for recommendations you might want to remove [gallery related subscriptions](https://github.com/Nosto/nosto-magento2/blob/master/etc/mview.xml#L45-L46). Modifying the `mview.xml`  file can be done for example using [Magento's patches](https://devdocs.magento.com/guides/v2.3/comp-mgr/patching.html). 

### Warning about `innodb_buffer_pool_size`

You will most likely see this warning in your Magento logs if you've installed MySQL using the defaults. To get rid of this warning we recommend increasing `innodb_buffer_pool_size` on you MySQL server configuration. You can find more info about indexer optimization from [the official Magento documentation](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/indexer-batch.html).

### Products not synchronized to Nosto

If the product data is not synchronized to Nosto you must verify the message queue consumers `nosto_product_sync.update` and `nosto_product_sync.delete` are running. Magento cron should take care of running \(and restarting if needed\) the consumers automatically but you can verify this by checking the process list \(`ps -ax` for example\) on your server.

You can also see the amount of products that are out of sync \("Products Out Of Sync"\) or needs to be rebuilt \("Products Marked As Dirty"\) in Magento's Nosto settings \(Stores &gt; Configuration &gt; Services &gt; Nosto\).

[![Configuration\_\_\_Settings\_\_\_Stores\_\_\_Magento\_Admin](https://user-images.githubusercontent.com/15191701/67567284-4f28a500-f732-11e9-976d-1c587d317b45.png)](https://user-images.githubusercontent.com/15191701/67567284-4f28a500-f732-11e9-976d-1c587d317b45.png)

### Bulk attribute updates not synchronized to Nosto

If you have the indexers running on mode "Update by save" the bulk operations are not automatically reflected to Nosto. This is due to how Magento processes bulk updates internally.

It is highly recommended to run all indexers in mode "Update by schedule".

### Nosto indexer is blocking other Magento indexers 

![](https://img.shields.io/badge/nosto-4.0.0%20%3C%205.0.0-green)

If the indexing process is too slow even after [parallelising the indexing process](https://github.com/Nosto/nosto-magento2/wiki/Indexer-troubleshooting#indexer-is-not-keeping-up-with-product-updates) you can set the product cache building process to be ran in a cron job. This way the sometimes heavy operation of building product cache would not affect other Magento indexers. Please keep in mind that if you set the product building to be done in a cron job you cannot utilise the parallelisation and the product synchronisation to Nosto will be slower due to this.

You can switch the product cache to to a cron from Nosto module's settings \(Stores &gt; Services &gt; Nosto\).

[![Configuration\_\_\_Settings\_\_\_Stores\_\_\_Magento\_Admin](https://user-images.githubusercontent.com/15191701/72806875-c7820200-3c5e-11ea-905a-afb06363e6c9.png)](https://user-images.githubusercontent.com/15191701/72806875-c7820200-3c5e-11ea-905a-afb06363e6c9.png)

### 

