![4.0.0](https://img.shields.io/badge/nosto-4.0.0-green.svg)

Nosto module (4.0.0) for Magento 2 introduced two brand new indexers to the existing Magento indexers. These indexers are used to drastically improve the performance of synchronising the product catalog with Nosto as well as generating the product tagging for product detail pages.

The indexers can be found by navigating to the Index Management view in the Magento settings.

The two indexers are named `InvalidateIndexer` and `DataIndexer`.

The main focus of the first indexer is to listen for product changes from Magento. In case a product is updated, the indexer will sign the product as dirty in the [cached product table](https://github.com/Nosto/nosto-magento2/wiki/Caching-Improvements).

The second indexer will listen for changes inside the product cache table itself. When a table entry is set to dirty, the DataIndexer will check and compare that the product has changed.

The product will be rebuilt, serialised and stored in the product_data field. Then the product is set as out of sync, which means that the data should be sent to Nosto through our APIs.

To further optimise the process, the module makes use of [message queues](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/message-queues.html).

All the products that have been rebuilt will be divided into batches and passed as messages to queue processor which will take care of sending the data and set the product as in_sync after it's a success.

## Manually Running the Indexer

You can run a full reindex of the product catalog by using Magento's built-in CLI indexer. 
After version 4.0.0, Nosto extension makes use of two indexers.

To reindex all products, re-run both indexers:
 - Invalidate indexer
```bash
bin/magento indexer:reset nosto_index_product_invalidate
bin/magento indexer:reindex nosto_index_product_invalidate
```

- Product data indexer
```bash
bin/magento indexer:reset nosto_index_product_data
bin/magento indexer:reindex nosto_index_product_data
```

## Indexer Parallelisation

Starting with version 2.2.6 Magento [supports parallel reindexing](https://community.magento.com/t5/Magento-DevBlog/Indexers-parallelization-and-optimization/ba-p/104922). Nosto's indexers support parallelization and both the Nosto indexers can be executed in parallel mode. The indexers are scoped based on stores. This means that if a merchant has n-stores, there will be n-processes running in parallel, each indexing a specific store (also called a "Dimension").

There are a few steps to be taken before enabling parallelisation:

1. Check the dimension mode for the indexer

```shell
bin/magento indexer:show-dimensions-mode
Product Price:                                     none
Nosto Product Index Data :                         none
Nosto Product Data Invalidator:                    none
```

2. Set the indexer mode for **both** to `store`

```shell
bin/magento indexer:set-dimensions-mode nosto_index_product_data store
Dimensions mode for indexer "Nosto Product Index Data " was changed from 'none' to 'store'

```

```shell
bin/magento indexer:set-dimensions-mode nosto_index_product_invalidate store
Dimensions mode for indexer "Nosto Product Data Invalidator" was changed from 'none' to 'store'
```

3. Make sure that the number of threads declared in the env variable `MAGE_INDEXER_THREADS_COUNT` is equal to the max number of stores.

For testing purposes, it can be declared in the CLI, like:

```shell
MAGE_INDEXER_THREADS_COUNT=3 php -f bin/magento indexer:reindex nosto_index_product_data
```


## Best Practices

We recommend the following best practices for Nosto indexers.

* We strongly advise that both indexer modes are set to `Update by Schedule` for better performance.  This will also make the indexers independent from each other.
* If you have multiple store views, we recommend that you enable multi-dimensional indexing for **both** indexers.


## Troubleshoot

For troubleshooting the indexer please follow the [troubleshoot wiki](https://github.com/Nosto/nosto-magento2/wiki/Indexer-troubleshooting).

If you are looking for instructions for the old indexer you can still find it from [here](https://github.com/Nosto/nosto-magento2/wiki/Indexer-(legacy)).