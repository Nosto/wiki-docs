# On 3.x

**This article only applies to the old version &lt; 4.0.0 of Nosto module**

Nosto for Magento adds a customer indexer to the existing Magento indexer. We highly encourage you to upgrade Nosto module to version &gt;= 4.0.0.

[![2.3.0](https://camo.githubusercontent.com/a5ab930572e73488f3e1afef1f234a4949c150a9/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6e6f73746f2d322e332e30253230253343253230342e302e302d677265656e2e737667)](https://camo.githubusercontent.com/a5ab930572e73488f3e1afef1f234a4949c150a9/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6e6f73746f2d322e332e30253230253343253230342e302e302d677265656e2e737667)

The indexer can be used drastically improve the performance of synchronizing the product catalog with Nosto. The indexer can be found by navigating to the **Index Management** view in the Magento settings.

[![](https://user-images.githubusercontent.com/327432/35035565-75d26b60-fb7a-11e7-80b9-6759a38cab6c.png)](https://user-images.githubusercontent.com/327432/35035565-75d26b60-fb7a-11e7-80b9-6759a38cab6c.png)

### Indexing Mode: Update On Save

We recommend you keep your indexer mode to be "Update on Save" and this is the default mode.

When the indexer is run in this mode, all product updates are instantaneously sent to Nosto. In the event that there is a noticeable performance impact, switch your indexer to the other mode.

### Indexing Mode: Update By Schedule

If you have a large product catalogue or a high-traffic site, we recommend you switch the indexer mode to be "Update by Schedule".

When the indexer is run in this mode, all product updates are not instantaneously sent to Nosto, but instead deferred until the next cron execution.

### Running the Indexer manually

You can run a full reindex of the product catalog by using Magento's built-in CLI indexer. To reindex all products, run:

```text
bin/magento indexer:reset nosto_product_sync
bin/magento indexer:reindex nosto_product_sync
```

If you update your store product periodically \(from some ERP system for example\) it's a good idea to run the indexer manually after the product update. Also, if you use catalog price rules and your product catalog is large you should run Nosto indexer periodically.

## Troubleshoot

If you are having issues with indexing you want to first enable Magento's debug logging [https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html](https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html). This will enable more verbose logging for the indexing. You will find indexing related logs from debug log \(`debug.log` by default\). All log entries are prefixed with "nosto".‌

We highly recommend that you upgrade your Nosto Magento module to the most recent version. This version is no longer maintained or supported. 

### Warning about `innodb_buffer_pool_size` <a id="warning-about-innodb_buffer_pool_size"></a>

You will most likely see this warning in your Magento logs if you've installed MySQL using the defaults. To get rid of this warning we recommend increasing `innodb_buffer_pool_size` on you MySQL server configuration. You can find more info about indexer optimization from [the official Magento documentation](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/indexer-batch.html).‌

We highly recommend updating your module version to [&gt; 5.0.0](on-5.x.md).

