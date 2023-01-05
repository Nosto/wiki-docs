![4.0.0](https://img.shields.io/badge/nosto-4.0.0-green.svg)

If you are having issues with indexing you want to first enable Magento's debug logging https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html. This will enable more verbose logging for the indexing. You will find indexing related logs from debug log (`debug.log` by default). All log entries are prefixed with "nosto".

## Indexer is not keeping up with product updates
If you are frequently updating massive amount of products (for example via API or import) there's a chance that the indexer cannot process the previous update before the next update batch is executed. In these cases we recommend [parallelising the indexer](https://github.com/Nosto/nosto-magento2/wiki/Product-data-indexing-and-caching#indexer-parallelisation).        

## Data indexer is blocking other Magento indexers
If the data indexing process is too slow even after [parallelising the indexing process](https://github.com/Nosto/nosto-magento2/wiki/Indexer-troubleshooting#indexer-is-not-keeping-up-with-product-updates) you can set the product cache building process to be ran in a cron job. This way the sometimes heavy operation of building product cache would not affect other Magento indexers. Please keep in mind that if you set the product building to be done in a cron job you cannot utilise the parallelisation and the product synchronisation to Nosto will be slower due to this. 

You can switch the product cache to to a cron from Nosto module's settings (Stores > Services > Nosto). 

![Configuration___Settings___Stores___Magento_Admin](https://user-images.githubusercontent.com/15191701/72806875-c7820200-3c5e-11ea-905a-afb06363e6c9.png)

## Warning about `innodb_buffer_pool_size`
You will most likely see this warning in your Magento logs if you've installed MySQL using the defaults. To get rid of this warning we recommend increasing `innodb_buffer_pool_size` on you MySQL server configuration. You can find more info about indexer optimization from [the official Magento documentation](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/indexer-batch.html). 

## Products not synchronized to Nosto 
If the product data is not synchronized to Nosto you must verify the message queue consumer `nosto_product_sync.update` is running. Magento cron should take care of running (and restarting if needed) the consumers automatically but you can verify this by checking the process list (`ps -ax` for example) on your server. 

**Note:** message queue consumer `nosto_product_sync.delete` was removed starting from version 7.2.0

You can also see the amount of products that are out of sync ("Products Out Of Sync") or needs to be rebuilt ("Products Marked As Dirty") in Magento's Nosto settings (Stores > Configuration > Services > Nosto).
 
![Configuration___Settings___Stores___Magento_Admin](https://user-images.githubusercontent.com/15191701/67567284-4f28a500-f732-11e9-976d-1c587d317b45.png)

## Bulk attribute updates not synchronized to Nosto
If you have the indexers running on mode "Update by save" the bulk operations are not automatically reflected to Nosto. This is due to how Magento processes bulk updates internally. 

It is highly recommended to run all indexers in mode "Update by schedule".
