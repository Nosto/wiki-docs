# Magento 2 indexer and Mview
In Nosto extension version 2.3.0 we introduced a custom indexer. Custom indexer will replace the event observers and console command for synchronizing product data to Nosto.

### Basic idea of a Magento indexer
Indexer (as the name implies) does data indexing. In Nosto's case it indexes product data to Nosto. In practise it sends the product data to Nosto via API.

If you set the indexer to be run "Update on save" the behaviour is pretty much the same than in previous versions of the extension. That is the product will updated immediately to Nosto via API. This can however cause performance issues if you have lot of product updates or you do product batch updates. 

In case you're doing lot of product updates or batch updates we suggest to set the Nosto Product Update indexer mode to "Update by schedule". When the indexer mode is "Update by schedule" Magento cron job will handle updating the indexes and with this mode the indexer can utilize batch updates.         

All indexers and their mode (including Nosto indexer) can be found from store admin “System” > “Index management”. More tech details about the indexer and mview can be found here: http://devdocs.magento.com/guides/v2.1/extension-dev-guide/indexing.html

![index_management___tools___system___magento_admin](https://user-images.githubusercontent.com/15191701/29363535-0f27b212-8299-11e7-9f31-ff9b5dd9a922.png)

### Advanced pricing (Magento 2 Community Edition)
Magento Community Edition's advanced (scheduled) pricing doesn't trigger the indexer when a scheduled price becomes active. Nosto extension updates products having advanced pricing via cron job. For this reason you must have [Magento Cron](http://devdocs.magento.com/guides/v2.1/config-guide/cli/config-cli-subcommands-cron.html) running on your server.


