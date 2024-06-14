# Using MySQL As Queue Message Broker

**NOTE:** From 7.6.0 the extension uses RabbitMQ as a default message broker

Nosto module uses [Magento's Bulk Operations](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/bulk-operations.html) and [Message Queues](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/message-queues.html) for rebuilding the product data, populating the product cache and synchronising the product data to Nosto over API. By default the message queues are backed by RabbitMQ, but Magento still supports using MySQL.

### Configuring MySQL for Message Queues

In order to make Nosto module to use MySQL for message queue processing, you need to override the message queue configuration files under the Nosto module. You must define the value of `connection` attribute to be `db` instead of `amqp`  to the following files. You might also want rename the `exchange` across the configurations files to something else than `magento-amqp`, like `magento-db`, for example.

* [etc/queue\_consumer.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_consumer.xml) 
* [etc/queue\_publisher.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_publisher.xml)
* [etc/queue\_topology.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_topology.xml)

After the configuration files have been overridden you must run `bin/magento setup:upgrade`.

For overriding the message queue configuration you can use for example [Magento's patches.](https://devdocs.magento.com/guides/v2.3/comp-mgr/patching.html) 

