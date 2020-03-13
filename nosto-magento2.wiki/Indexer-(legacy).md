**This article only applies to the old version < 4.0.0 of Nosto module**

Nosto for Magento adds a customer indexer to the existing Magento indexer. We highly encourage you to upgrade Nosto module to version >= 4.0.0.


![2.3.0](https://img.shields.io/badge/nosto-2.3.0%20%3C%204.0.0-green.svg)

The indexer can be used drastically improve the performance of synchronizing the product catalog with Nosto. The indexer can be found by navigating to the **Index Management** view in the Magento settings.

![](https://user-images.githubusercontent.com/327432/35035565-75d26b60-fb7a-11e7-80b9-6759a38cab6c.png)

## Indexing Mode: Update On Save

We recommend you keep your indexer mode to be "Update on Save" and this is the default mode.

When the indexer is run in this mode, all product updates are instantaneously sent to Nosto. In the event that there is a noticeable performance impact, switch your indexer to the other mode.

## Indexing Mode: Update By Schedule

If you have a large product catalogue or a high-traffic site, we recommend you switch the indexer mode to be "Update by Schedule".

When the indexer is run in this mode, all product updates are not instantaneously sent to Nosto, but instead deferred until the next cron execution.

## Running the Indexer manually

You can run a full reindex of the product catalog by using Magento's built-in CLI indexer. To reindex all products, run:

```shell
bin/magento indexer:reset nosto_product_sync
bin/magento indexer:reindex nosto_product_sync
```

If you update your store product periodically (from some ERP system for example) it's a good idea to run the indexer manually after the product update. Also, if you use catalog price rules and your product catalog is large you should run Nosto indexer periodically. 