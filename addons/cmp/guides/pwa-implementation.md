# PWA Implementation

It is now possible to use the category merchandising feature in PWA.  
The merchandising rules are applied to the catalog by using the `nosto_personalized` sorting option in products grapqhl query. It comes out of the box from version `>3.1.0` and it does not require any further configuration.

## Filters

CMP module is compatible with Magento 2 GraphQl filtering. You can filter through:

* Price
* Custom attributes (color, size etc.)
* Brand

## Query example

```text
{
  products(
    filter: { 
      category_id: { eq: "4" },
      price: {
        from: "10",
        to: "100"
      },
      color: {
        in: [ "49", "50" ]
      },
      eco: {
        eq: true
      }
    }
    currentPage: 1
    pageSize: 5
    sort: {nosto_personalized: DESC}
  ) {
    total_count
    items {
      name
      sku
      price_range {
        minimum_price {
          regular_price {
            value
            currency
          }
        }
      }
    }
    page_info {
      page_size
      current_page
    }
  }
}
```

