# Indexer

![](https://img.shields.io/badge/nosto-%3E%3D%205.0.0-green)

Indexer logic changed in \(5.0.0\) in order to avoid blocking other indexing processing. As of 5.0.0 the product data is building is done in [bulk operations](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/bulk-operations.html).

Version &gt; 5.0.0 has two indexers `Nosto Product Queue`_\(nosto\_index\_product\_queue\)_ and `Nosto Product Queue Processor`_\(nosto\_index\_product\_queue\_processor\)_.  The first indexer \(_nosto\_index\_product\_queue_\) listens for product changes in Magento and adds the changed product ids into a queue. The second indexer \(_nosto\_index\_product\_queue\_processor\)_ fetches the product ids from the queue, merges the queues when possible, removes duplicated product ids and sends the product ids to the bulk operation that builds the product data and sends the data to Nosto. [The cache](../product-data-caching/built-in-caching.md) is also updated in bulk operations.  

To further optimise the process, the bulk operations can be configured to use [message queues](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/message-queues.html).

## Manually Running the Indexer

You can run a full reindex of the product catalog by using Magento's built-in CLI indexer.

To reindex all products, re-run both indexers:

* _nosto\_index\_product\_queue_ indexer

  ```bash
  bin/magento indexer:reset nosto_index_product_queue
  bin/magento indexer:reindex nosto_index_product_queue
  ```

* _nosto\_index\_product\_queue\_processor_

  ```bash
  bin/magento indexer:reset nosto_index_product_queue_processor
  bin/magento indexer:reindex nosto_index_product_queue_processor
  ```

## Best Practices

We recommend the following best practices for Nosto indexers.

* We strongly advise that both indexer modes are set to `Update by Schedule` for better performance.  This will also make the product updates to Nosto more reliable. For example the scheduled catalog price rules would not be updated in real-time to Nosto unless the indexer mode is set to  `Update by Schedule` 
* If you have multiple store views, we recommend that you enable multi-dimensional indexing for **both** indexers.

## Troubleshoot

For troubleshooting the indexer please follow the [troubleshoot wiki](indexer-troubleshooting.md).

If you are looking for instructions for the old indexer \(&lt; 5.0.0\) you can still find it from [here](4.0.0-less-than-5.0.0.md).

