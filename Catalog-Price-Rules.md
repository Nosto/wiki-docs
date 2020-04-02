![3.5.0](https://img.shields.io/badge/nosto-3.5.0-green.svg)

Nosto's Magento extension can be configured to update product prices to Nosto in real time when catalog price rules are updated. By default catalog price rule changes are not updated to Nosto in real time since it's quite a heavy operation.

You can enable automatic catalog price rule updates from the Advanced settings of Nosto's Magento extension.

<img width="1171" alt="configuration___system___magento_admin" src="https://user-images.githubusercontent.com/15191701/37512774-bc410c50-290b-11e8-8506-32620a067282.png">

## Automatic catalog rule updates enabled
When automatic catalog rule updates are enabled Nosto will update all products matching any of the catalog price rules immediately after the rules have been applied. If you have a large product catalog you most likely don't want to enable automatic catalog rule updates. Instead you should enable Nosto indexer and run the indexer periodically (Indexer#running-indexer-manually).

## Automatic catalog rule updates disabled
If automatic catalog rule updates are disabled Nosto will index the new prices whenever a product is updated in Magento, indexer is ran manually or via Nosto's normal product update process (https://help.nosto.com/settings-and-troubleshooting-faq/tools-faq/understanding-the-product-update-and-creation-process).