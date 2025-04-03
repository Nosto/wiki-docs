---
description: >-
  This documentation explains how to implement Nosto tagging on headless
  frontends, PWA implementations, or custom themes that are neither Luma nor
  Hyva.
---

# Tagging Providers

### Table of Contents

* [Introduction](tagging-providers.md#introduction)
* [Architecture Overview](tagging-providers.md#architecture-overview)
* [Data Structure](tagging-providers.md#data-structure)
* [Integration Methods](tagging-providers.md#integration-methods)
  * [Method 1: REST API](tagging-providers.md#method-1-rest-api)
  * [Method 2: GraphQL](tagging-providers.md#method-2-graphql)
  * [Method 3: Direct JavaScript Implementation](tagging-providers.md#method-3-direct-javascript-implementation)
* [Implementing Tagging Providers](tagging-providers.md#implementing-tagging-providers)
* [Testing Your Implementation](tagging-providers.md#testing-your-implementation)
* [Troubleshooting](tagging-providers.md#troubleshooting)

### Introduction

Nosto's tagging providers are a flexible way to send data to Nosto without requiring the traditional DOM-based tagging approach. This method is particularly useful for headless implementations, PWAs, or any frontend that doesn't follow Magento's traditional rendering approach.

### Architecture Overview

The tagging provider system consists of three main components:

1. **Data Generation**: Magento backend prepares structured data for Nosto
2. **Data Delivery**: Methods to deliver this data to the frontend
3. **Provider Registration**: JavaScript that registers the data with Nosto

### Data Structure

For more details, refer to the Nosto API documentation: [https://nosto.github.io/nosto-js/interfaces/client.TaggingData.html](https://nosto.github.io/nosto-js/interfaces/client.TaggingData.html)

The tagging provider data follows this basic structure:

```
{
  "pageType": "product", // product, category, frontpage, cart, search, notfound, order
  "products": [...],     // array of product objects when on product page
  "cart": {...},         // cart data
  "customer": {...},     // customer data
  "categories": [...],   // array of category paths
  "variation": "...",    // pricing variation or currency information
  "searchTerm": "..."    // on search pages
}
```

### Integration Methods

#### Method 1: REST API

You can create a custom endpoint to retrieve the tagging data:

1. **Create an API endpoint** in your Magento module:

```
<?php
namespace Vendor\Module\Api;

interface NostoTaggingInterface
{
    /**
     * Get tagging data for current context
     *
     * @param string $pageType
     * @return array
     */
    public function getTaggingData($pageType);
}
```

2. **Implement the service**:

```
<?php
namespace Vendor\Module\Model;

use Nosto\Tagging\Block\TaggingProvider;
use Magento\Framework\App\RequestInterface;

class NostoTaggingService implements \Vendor\Module\Api\NostoTaggingInterface
{
    private $taggingProvider;
    private $request;
    
    public function __construct(
        TaggingProvider $taggingProvider,
        RequestInterface $request
    ) {
        $this->taggingProvider = $taggingProvider;
        $this->request = $request;
    }
    
    public function getTaggingData($pageType)
    {
        // Set page type from API parameter
        $this->taggingProvider->setData('page_type', $pageType);
        
        // Return the tagging configuration
        return $this->taggingProvider->getTaggingConfig();
    }
}
```

3. **Register in di.xml**:

```
<preference for="Vendor\Module\Api\NostoTaggingInterface" 
            type="Vendor\Module\Model\NostoTaggingService" />
```

4. **Define in webapi.xml**:

```
<route url="/V1/nosto/tagging/:pageType" method="GET">
    <service class="Vendor\Module\Api\NostoTaggingInterface" method="getTaggingData"/>
    <resources>
        <resource ref="anonymous"/>
    </resources>
</route>
```

#### Method 2: GraphQL

For PWA implementations, GraphQL is often preferred:

1. **Create schema.graphqls**:

```
type Query {
    nostoTaggingData(
        pageType: String!
    ): NostoTaggingData @resolver(class: "Vendor\\Module\\Model\\Resolver\\NostoTaggingResolver")
}

type NostoTaggingData {
    pageType: String
    products: [NostoProduct]
    cart: NostoCart
    customer: NostoCustomer
    categories: [String]
    variation: String
    searchTerm: String
}

# Define other required types (NostoProduct, NostoCart, etc.)
```

2. **Create the resolver**:

```
<?php
namespace Vendor\Module\Model\Resolver;

use Magento\Framework\GraphQl\Config\Element\Field;
use Magento\Framework\GraphQl\Query\ResolverInterface;
use Magento\Framework\GraphQl\Schema\Type\ResolveInfo;
use Nosto\Tagging\Block\TaggingProvider;

class NostoTaggingResolver implements ResolverInterface
{
    private $taggingProvider;
    
    public function __construct(TaggingProvider $taggingProvider)
    {
        $this->taggingProvider = $taggingProvider;
    }
    
    public function resolve(
        Field $field,
        $context,
        ResolveInfo $info,
        array $value = null,
        array $args = null
    ) {
        $pageType = $args['pageType'] ?? 'unknown';
        $this->taggingProvider->setData('page_type', $pageType);
        
        return $this->taggingProvider->getTaggingConfig();
    }
}
```

#### Method 3: Direct JavaScript Implementation

If you're building a non-Magento frontend but using Magento's backend, you can include the Nosto tagging-providers.js script directly:

```
<script src="/path/to/nostojs.js"></script>
<script src="/path/to/tagging-providers.js"></script>
<script>
  // Fetch data from your custom API/GraphQL
  fetch('/api/v1/nosto-data?pageType=product')
    .then(response => response.json())
    .then(data => {
      // Initialize tagging providers with the data
      window.initNostoTaggingProviders(data);
    });
</script>
```

### Implementing Tagging Providers

In your frontend application, after retrieving the data, you need to register the tagging providers:

```
function initTagging(taggingConfig) {
  // Wait for nostojs to be available
  function waitForNostoJs(callback, maxAttempts = 50, interval = 100) {
    if (typeof nostojs === 'function') {
      callback();
      return;
    }

    let attempts = 0;
    const checkNostoJs = setInterval(function() {
      attempts++;

      if (typeof nostojs === 'function') {
        clearInterval(checkNostoJs);
        callback();
        return;
      }

      if (attempts >= maxAttempts) {
        clearInterval(checkNostoJs);
        console.log('Failed to load nostojs after ' + maxAttempts + ' attempts');
      }
    }, interval);
  }

  // Register the tagging providers
  function setupTaggingProviders() {
    nostojs(function (api) {
      // Register the page type
      api.internal.setTaggingProvider("pageType", function () {
        return taggingConfig.pageType;
      });

      // Register products if available
      if (taggingConfig.products) {
        api.internal.setTaggingProvider("products", function () {
          return taggingConfig.products;
        });
      }

      // Register cart if available
      if (taggingConfig.cart) {
        api.internal.setTaggingProvider("cart", function () {
          return taggingConfig.cart;
        });
      }

      // Register customer if available
      if (taggingConfig.customer) {
        api.internal.setTaggingProvider("customer", function () {
          return taggingConfig.customer;
        });
      }

      // Register categories if available
      if (taggingConfig.categories) {
        api.internal.setTaggingProvider("categories", function () {
          return taggingConfig.categories;
        });
      }

      // Register variation if available
      if (taggingConfig.variation) {
        api.internal.setTaggingProvider("variation", function () {
          return taggingConfig.variation;
        });
      }

      // Register search term if available
      if (taggingConfig.searchTerm) {
        api.internal.setTaggingProvider("searchTerm", function () {
          return taggingConfig.searchTerm;
        });
      }
    });
  }

  waitForNostoJs(setupTaggingProviders);
}
```

### Testing Your Implementation

1. **Verify data structure**: Ensure your API returns properly formatted data
2. **Check Nosto Debug Toolbar**: Enable the Nosto Debug Toolbar to verify data is being sent
3. **Validate with Nosto Dashboard**: Check if data appears in the Nosto dashboard
4. **Test recommendations**: Ensure recommendations appear on your pages

### Troubleshooting

Common issues and solutions:

| Issue                             | Solution                                                        |
| --------------------------------- | --------------------------------------------------------------- |
| Nosto script not loading          | Make sure the Nosto script is included before tagging providers |
| Data not appearing                | Check browser console for errors, verify data structure         |
| Recommendations not personalizing | Ensure customer data is correctly formatted                     |
| Multiple currencies not working   | Check variation ID is properly passed                           |

For more detailed assistance, consult the Nosto Support team or refer to the [official Nosto documentation](https://docs.nosto.com/).
