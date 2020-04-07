# Supplier Cost & Inventory Level

Nosto supports supplier costs and inventory level. The supplier-cost and inventory-level metadata can be used for creating advanced recommendation rules to promote products with a high margin or low-stock products.

![2.10.0](https://img.shields.io/badge/nosto-2.10.0-green.svg)

The supplier-cost and inventory-level of a product are considered sensitive information and therefore does not exist in the page tagging, and instead, are sent over a secure API.

### Why don't all my products have the supplier-cost and inventory-level information?

The extension relies on observers to detect changes to the catalog and synchronize it with Nosto. If you are synchronizing products from your ERP/PIM system, observers may not be triggered, and therefore it is necessary that you run a cron to bulk update products to Nosto.

→ Learn more about [Bulk Product Updates](../guides/bulk-product-updates.md)

## Enabling / Disabling Inventory-Level Metadata

The inventory-level metadata collection is disabled by default. You can toggle the inventory-level metadata collection by navigating to the configuration page and toggling **Send inventory level to Nosto** under the **Advanced settings** section.

![Inventory-Level Metadata Settings](https://user-images.githubusercontent.com/327432/31169781-780ffbe2-a902-11e7-8c10-763c0c560b48.png)

## Enabling / Disabling Inventory-Level Sync After Purchase

In some cases, syncing the inventory level after a purchase might add some loading time to the `thank you` page. As of version `3.7.8`, you can disable it and leave this operation for a later time when running the indexer or updating the product manually.

![configuration\_\_\_system\_\_\_magento\_admin](https://user-images.githubusercontent.com/2778820/50331528-a143dd00-0507-11e9-9b21-674e7746f4b5.png)

## Defining the attribute for supplier cost

You can define the attribute to be used for supplier cost from the extension's settings under `Optional attributes`.

![Configuration\_\_\_System\_\_\_Magento\_Admin](https://user-images.githubusercontent.com/15191701/69333792-0fd27300-0c62-11ea-9dbf-7aaf2f78891c.png)

