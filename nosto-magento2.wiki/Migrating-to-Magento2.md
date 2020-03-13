If you are in the process of migrating to Magento2, there are few things to be kept in mind. Follow the installation guide for instructions on setting up Nosto for Magento2.

### Setting up the account

In some cases, you may reuse your existing Nosto account but if your account id is prefixed with a platform such as `prestashop-`, `shopify-`, `shopware-`, you will need to create a new account. Also, if you are migrating from Magento 1 to Magento 2 we strongly recommend creating a new Nosto account. Otherwise,
 there's a good chance that the product identifiers are mixed and Nosto would recommend weird products based on the old product identifiers.   

**Note:** You must remember to change your domain name in the Nosto backend to point to the new site.

### Checking your catalog

Prior to migrating to M2, you should check your product catalog for the following pitfalls.

#### Product IDs

The product identifiers must be the same after migration. If your product-ids are different, you will lose all the relational data. Nosto uses the product identifier to uniquely identify a product and it is that particular identifier which is the key to all relations.

#### Product URLs

Product URLs should stay the same in Magento 2 after the migration if possible. If you cannot prevent product URLs from changing you should apply _Moved Permanently (301)_ HTTP header for the old product URLs and redirect visitors to the new product URL.

**Note:** Handling the product URLs correctly is not crucial only for Nosto but for SEO point of view as well.

### Synchronising the Product Catalog

If you are reusing an existing account, request our support personnel to initiate a full reindex of your product catalog. This process will eliminate all dead links. If the product URLs have changed due to the migration, Nosto will automatically detect new products and update its catalog.

### Handling old and new orders

Any old orders tracked by Nosto will remain as-is any new orders will be handled automatically. This requires no action from you. 

**Note:** Make sure to use at least version 2.7.0 of Nosto's Magento 2 extension. This version prefixes the order numbers to avoid order number collision with the previous platform.