From this article you can find a checklist what to take into account when you upgrade to Nosto module version >= 4.0.0.

### Upgrade only Nosto module

It's recommended not to upgrade any other modules at the same time when upgrading Nosto module to version >= 4.0.0. If other module upgrades alter the product data it might lead into a full re-index of Nosto product data during the module upgrade process. With large catalog and multiple store views the full re-index might take hours to complete.

### Run the setup commands

As with any Magento 2 module upgrade remember to run `bin/magento setup:upgrade` and `bin/magento setup:di:compile` commands. Note that these might be automated by your deployment flow.

### Set your indexers to "Update by Schedule"

We introduced two new indexers along with version 4.0.0. It's highly recommended to set the indexer mode to be "Update by Schedule" for both of the indexers. Read more about the new indexers [here](https://github.com/Nosto/nosto-magento2/wiki/Indexer)

### Re-connect Nosto account

If you are upgrading from module version < 3.9.0 you must re-connect your Nosto account. This is due to the fact that order confirmations are also sent through Graphql API that requires an additional authentication token.

### Double check the overrides

As version 4.0.0 introduces breaking changes (renamed classes, new indexers, changes in XML configuration files, etc.) you must carefully amend possibly module overrides you have in place.