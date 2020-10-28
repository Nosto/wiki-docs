## Implementing CMP on Magento2 PWA

It is now possible to use the category merchandising feature in PWA.  
The merchandising rules are applied to the catalog by using the `nosto_personalized` sorting option 
in products query. It comes out of the box from version `>3.1.0` and it does not require any further
configuration.

### Filters

Currently the only filters supported for merchandising is the price filtering. 


### Query example

```
{
  products(
    filter: { 
    	category_id: { eq: "4" },
     	price: {
      	from:"10",
      	to: "100"
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

