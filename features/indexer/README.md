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

## Indexer Parallelisation

Starting with version 2.2.6 Magento [supports parallel reindexing](https://community.magento.com/t5/Magento-DevBlog/Indexers-parallelization-and-optimization/ba-p/104922). Nosto's indexers support parallelization and both the Nosto indexers can be executed in parallel mode. The indexers are scoped based on stores. This means that if a merchant has n-stores, there will be n-processes running in parallel, each indexing a specific store \(also called a "Dimension"\).

There are a few steps to be taken before enabling parallelisation:

#### Check the dimension mode for the indexer

```text
bin/magento indexer:show-dimensions-mode
Product Price:                                     none
Nosto Product Index Data :                         none
Nosto Product Data Invalidator:                    none
```

#### Set the indexer mode for **both** to `store`

```text
bin/magento indexer:set-dimensions-mode nosto_index_product_data store
Dimensions mode for indexer "Nosto Product Index Data " was changed from 'none' to 'store'
```

```text
bin/magento indexer:set-dimensions-mode nosto_index_product_invalidate store
Dimensions mode for indexer "Nosto Product Data Invalidator" was changed from 'none' to 'store'
```

Make sure that the number of threads declared in the env variable `MAGE_INDEXER_THREADS_COUNT` is equal to the max number of stores.

For testing purposes, it can be declared in the CLI, like:

```text
MAGE_INDEXER_THREADS_COUNT=3 php -f bin/magento indexer:reindex nosto_index_product_data
```

Nosto module uses [Magento's Bulk Operations](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/bulk-operations.html) and [Message Queues](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/message-queues.html) for rebuilding the product data, populating the product cache and synchronising the product data to Nosto over API. By default the message queues are backed by MySQL but Magento also supports [using RabbitMQ](https://devdocs.magento.com/guides/v2.3/install-gde/prereq/install-rabbitmq.html) for message queues.  

## Using Message Queues \(RabbitMQ\)

In order to make Nosto module to use RabbitMQ for message queue processing you need to override the message queue configuration files under Nosto module. You must define the value of `connection` attribute to be `amqp` instead of `db`  to  the following files. You might also want rename the `exchange` across the configurations files to something else than `magento-db`.

* [etc/queue\_consumer.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_consumer.xml) 
* [etc/queue\_publisher.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_publisher.xml)
* [etc/queue\_topology.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_topology.xml)

We recommend also deleting the file [queue.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue.xml) as it's only used when Message Queues are using MySQL.

After the configuration files have been overridden you must run `bin/magento setup:upgrade`.

For overriding the message queue configuration you can use for example [Magento's patches.](https://devdocs.magento.com/guides/v2.3/comp-mgr/patching.html) 

## Best Practices

We recommend the following best practices for Nosto indexers.

* We strongly advise that both indexer modes are set to `Update by Schedule` for better performance.  This will also make the product updates to Nosto more reliable. For example the scheduled catalog price rules would not be updated in real-time to Nosto unless the indexer mode is set to  `Update by Schedule` 
* If you have multiple store views, we recommend that you enable multi-dimensional indexing for **both** indexers.

## Troubleshoot

For troubleshooting the indexer please follow the [troubleshoot wiki](indexer-troubleshooting.md).

If you are looking for instructions for the old indexer \(&lt; 5.0.0\) you can still find it from [here](4.0.0-less-than-5.0.0.md).

