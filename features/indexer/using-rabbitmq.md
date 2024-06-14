# Using RabbitMQ

**NOTE:** From 7.6.0 the extension uses RabbitMQ as a default message broker, this manual must be used only by older extension versions.

Nosto module uses [Magento's Bulk Operations](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/bulk-operations.html) and [Message Queues](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/message-queues.html) for rebuilding the product data, populating the product cache and synchronising the product data to Nosto over API. By default the message queues are backed by MySQL but Magento also supports [using RabbitMQ](https://devdocs.magento.com/guides/v2.3/install-gde/prereq/install-rabbitmq.html) for message queues.  

### Configuring RabbitMQ for Message Queues

In order to make Nosto module to use RabbitMQ for message queue processing you need to override the message queue configuration files under Nosto module. You must define the value of `connection` attribute to be `amqp` instead of `db`  to  the following files. You might also want rename the `exchange` across the configurations files to something else than `magento-db`.

* [etc/queue\_consumer.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_consumer.xml) 
* [etc/queue\_publisher.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_publisher.xml)
* [etc/queue\_topology.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue_topology.xml)

We recommend also deleting the file [queue.xml](https://github.com/Nosto/nosto-magento2/blob/master/etc/queue.xml) as it's only used when Message Queues are using MySQL.

After the configuration files have been overridden you must run `bin/magento setup:upgrade`.

For overriding the message queue configuration you can use for example [Magento's patches.](https://devdocs.magento.com/guides/v2.3/comp-mgr/patching.html) 


