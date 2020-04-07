# Indexer

Nosto for Magento adds a customer indexer to the existing Magento indexer.

> NOTE: This feature has been removed from the extension after version 4.0.0

![3.5.0](https://img.shields.io/badge/nosto-3.5.0-green.svg)

The indexer can be used drastically improve the performance of synchronizing the product catalog with Nosto and also generating the product tagging in store front. The indexer can be found by navigating to the **Index Management** view in the Magento settings.

![](https://user-images.githubusercontent.com/327432/35354334-613df920-0152-11e8-9348-6d8703d2ad4d.png)

Nosto's product indexer uses Magento's built-in indexer logic. In practice, Nosto indexer saves a serialized Nosto product object into a database. When Magento indicates that product data has been changed Nosto indexer will save the product data into the Nosto index if product data significant to Nosto was changed. When the indexer is used, product API calls to Nosto are made only when Nosto product data has been changed.

## Indexing Mode: Update On Save

We recommend you keep your indexer mode to be "Update on Save" and this is the default mode.

When the indexer is run in this mode, all product updates are instantaneously sent to Nosto. In the event that there is a noticeable performance impact, switch your indexer to the other mode.

## Indexing Mode: Update By Schedule

If you have a large product catalog or a high-traffic site, we recommend you switch the indexer mode to be "Update by Schedule".

When the indexer is run in this mode, all product updates are not instantaneously sent to Nosto, but instead deferred until the next cron execution.

## Running the Indexer manually

You can run a full reindex of the product catalog by using Magento's built-in CLI indexer. To reindex all products, run:

```text
php ./shell/indexer.php --reindex nosto_indexer
```

If you update your store product periodically \(from some ERP system for example\) it's a good idea to run the indexer manually after the product update. Also, if you use catalog price rules and your product catalog is large you should run the indexer periodically.

## Enable indexing

Please note that product data indexing for Nosto products is disabled by default. You must enable the indexing from Nosto extension's settings in Magento.

![configuration\_\_\_system\_\_\_magento\_admin](https://user-images.githubusercontent.com/15191701/44197257-d64db680-a146-11e8-8de7-4a429a1a07b1.png)

## Limiting available memory for the indexer

See [https://github.com/Nosto/nosto-magento/wiki/Configuring\#indexer-memory](https://github.com/Nosto/nosto-magento/wiki/Configuring#indexer-memory)

