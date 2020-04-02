Nosto supports supplier costs and inventory level. The supplier-cost and inventory-level metadata can be used for creating advanced recommendation rules to promote products with a high margin or low-stock products.

![1.2.0](https://img.shields.io/badge/nosto-1.2.0-green.svg)

The supplier-cost and inventory-level of a product are considered sensitive information and therefore does not exist in the page tagging, and instead, are sent over a secure API.

#### Why don't all my products have the supplier-cost and inventory-level information?

The extension relies on observers to detect changes to the catalog and synchronize it with Nosto. If you are synchronizing products from your ERP/PIM system, observers may not be triggered.

## Enabling / Disabling Inventory-Level Metadata

The supplier-cost and inventory-level metadata collection is always on and does not have a corresponding toggle.