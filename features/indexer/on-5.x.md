# On 5.x

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

If you are having issues with indexing you want to first enable Magento's debug logging [https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html](https://devdocs.magento.com/guides/v2.3/config-guide/cli/logging.html). This will enable more verbose logging for the indexing. You will find indexing related logs from debug log \(`debug.log` by default\). All log entries are prefixed with "nosto".‌

### Indexer is not keeping up with product updates <a id="indexer-is-not-keeping-up-with-product-updates"></a>

‌If you are frequently updating massive amount of products \(for example via API or import\) there's a chance that the indexer cannot process the previous update before the next update batch is executed. In these cases we recommend [parallelising the indexer](https://app.gitbook.com/@nosto/s/magento-2/~/drafts/-M9Nt0el0H5wkhKBxY6J/features/indexer#indexer-parallelisation/@merged) as a first step.‌

We also recommend figuring out the source of frequent product updates and do optimisations for the [mview subscriptions / triggers](https://github.com/Nosto/nosto-magento2/blob/master/etc/mview.xml#L38). For example if you are using 3rd party module / integration that updates all product images frequently but those images are not used for recommendations you might want to remove [gallery related subscriptions](https://github.com/Nosto/nosto-magento2/blob/master/etc/mview.xml#L45-L46). Modifying the `mview.xml` file can be done for example using [Magento's patches](https://devdocs.magento.com/guides/v2.3/comp-mgr/patching.html).‌

### Warning about `innodb_buffer_pool_size` <a id="warning-about-innodb_buffer_pool_size"></a>

You will most likely see this warning in your Magento logs if you've installed MySQL using the defaults. To get rid of this warning we recommend increasing `innodb_buffer_pool_size` on you MySQL server configuration. You can find more info about indexer optimization from [the official Magento documentation](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/indexer-batch.html).‌

### Products not synchronized to Nosto <a id="products-not-synchronized-to-nosto"></a>

If the product data is not synchronized to Nosto you must verify the message queue consumers `nosto_product_sync.update` and `nosto_product_sync.delete` are running. Magento cron should take care of running \(and restarting if needed\) the consumers automatically but you can verify this by checking the process list \(`ps -ax` for example\) on your server.‌​[​](https://user-images.githubusercontent.com/15191701/67567284-4f28a500-f732-11e9-976d-1c587d317b45.png)

### Bulk attribute updates not synchronized to Nosto <a id="bulk-attribute-updates-not-synchronized-to-nosto"></a>

If you have the indexers running on mode "Update by save" the bulk operations are not automatically reflected to Nosto. This is due to how Magento processes bulk updates internally.‌

It is highly recommended to run all indexers in mode "Update by schedule".‌

