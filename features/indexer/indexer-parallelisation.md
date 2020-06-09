# Indexer Parallelisation

## Indexer Parallelisation

Starting with version 2.2.6 Magento [supports parallel reindexing](https://community.magento.com/t5/Magento-DevBlog/Indexers-parallelization-and-optimization/ba-p/104922). Nosto's indexers support parallelisation and both the Nosto indexers can be executed in parallel mode. The indexers are scoped based on stores. This means that if a merchant has n-stores, there will be n-processes running in parallel, each indexing a specific store \(also called a "Dimension"\).

### Enabling parallelisation for the indexer

There are a few steps to be taken before enabling parallelisation. Begin by checking the current dimension modes for the indexer.

```text
bin/magento indexer:show-dimensions-mode
Product Price:                                     none
Nosto Product Queue:                               none
Nosto Product Queue Processor:                     none
```

Change the indexer mode to `store`. This will force each store to be processed independently.

```text
bin/magento indexer:set-dimensions-mode nosto_index_product_queue store
Dimensions mode for indexer "Nosto Product Queue " was changed from 'none' to 'store'
```

```text
bin/magento indexer:set-dimensions-mode nosto_index_product_queue_processor store
Dimensions mode for indexer "Nosto Product Queue Processor" was changed from 'none' to 'store'
```

You may need to tweak the number of threads declared in the env variable `MAGE_INDEXER_THREADS_COUNT` is equal to the max number of stores.

 For testing purposes, it can be declared in the CLI, like:

```text
MAGE_INDEXER_THREADS_COUNT=3 php -f bin/magento indexer:reindex nosto_index_product_queue
```



