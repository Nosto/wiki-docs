# Implement Category pages

Nosto provides functionality to retrieve all products for a specific category. This is useful when you want to implement category merchandising using the same API as for Search.

## API Requests <a href="#autocomplete" id="autocomplete"></a>

### Using category ID and category path

{% hint style="info" %}
Using the category ID is only fully supported for Shopify merchants. Others should use the category path instead to benefit from full functionality.
{% endhint %}

Provide the [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) API parameter to fetch all products associated with that category. Additionally [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) should be provided for better analytics data.

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryId: "123456789",
      categoryPath: "Pants"
    }
  ) {
    products {
      hits {
        productId
        name
        url
        imageUrl
        price
      }
      total
      size
    }
  }
}
```

{% hint style="info" %}
Product fields that can be requested in the `hits` object are documented [here](https://search.nosto.com/v1/graphql?ref=SearchProduct). All indexed fields are accessible via the API.
{% endhint %}

### Using only category path

Provide the [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) API parameter to fetch all products associated with that category. This parameter is the same as the `categories` product field.

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryPath: "Pants"
    }
  ) {
    products {
      hits {
        productId
        name
        url
        imageUrl
        price
      }
      total
      size
    }
  }
}
```

{% hint style="info" %}
Product fields that can be requested in the `hits` object are documented [here](https://search.nosto.com/v1/graphql?ref=SearchProduct). All indexed fields are accessible via the API.
{% endhint %}

#### Child category handling

Depending on your configuration, fetching a parent category will also include products from the child categories. For example, fetching products for the category `Pants` would also include products from the categories `Pants -> Shorts` and `Pants -> Khakis`.

This is an admin-only setting. Please contact your Nosto representative to adjust this setting.

### Using custom filters

In some rare cases [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) or [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) is not enough. In these cases [custom filters](https://search.nosto.com/v1/graphql?ref=InputSearchFilter) can be used to build any query for category & landing pages.

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      preFilter: [
        {
          field: "productId",
          value: [
            "2276",
            "2274"
          ]
        }
      ],
    }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
    }
  }
}
```

### Other features & implementation

The category page shares a lot of similarities with the search page, so please refer to the search page documentation:

{% content-ref url="implementing-search-page.md" %}
[implementing-search-page.md](implementing-search-page.md)
{% endcontent-ref %}

## Analytics

### Nosto Analytics

To analyze user behavior you need to implement tracking. This can be achieved using our [JavaScript library](../search/). You need to implement the following methods with `type = category`:

* [recordSearch](../search/#search-1) to track category page visits
* [recordSearchClick](../search/#search-product-keyword-click) to track clicks on category results

## Fallback mechanism

Similar to search implementations, category merchandising via API requires fallback mechanisms to handle errors and timeouts gracefully.

### Implementation guidance

When the category API call returns an error or takes longer than 1 second to respond, the integration should fall back to the native category page solution:

```javascript
async function loadCategoryWithFallback(categoryId, categoryPath) {
    const timeout = 1000; // 1 second timeout
    
    try {
        const timeoutPromise = new Promise((_, reject) => {
            setTimeout(() => reject(new Error('Category API timeout')), timeout);
        });
        
        const categoryPromise = fetch(`https://search.nosto.com/v1/graphql`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${YOUR_TOKEN}`
            },
            body: JSON.stringify({
                query: `
                    query {
                        search(
                            accountId: "${YOUR_ACCOUNT_ID}"
                            products: {
                                categoryId: "${categoryId}",
                                categoryPath: "${categoryPath}"
                            }
                        ) {
                            products {
                                hits {
                                    productId
                                    name
                                    url
                                    imageUrl
                                    price
                                }
                                total
                            }
                        }
                    }
                `
            })
        }).then(response => response.json());
        
        const result = await Promise.race([categoryPromise, timeoutPromise]);
        return result;
        
    } catch (error) {
        console.warn('Nosto category API failed, using native category logic:', error);
        // Fallback to native category page implementation
        return await loadNativeCategoryProducts(categoryId, categoryPath);
    }
}
```

This ensures users always see category products, even when the Nosto API is unavailable or slow to respond.

