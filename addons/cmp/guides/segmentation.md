# Segmentation in Category Merchandising

Nosto offers the possibility to deliver category merchandising based on different segments. The segments can be [configured in Nosto](https://help.nosto.com/en/articles/3067796-segmentation-targeting-options).

One challenge to deliver segmentation is to overcome the Magento cache so different segments will not be served the same sorting. 
The segmentation logic is handled by the Nosto client script. Nosto's CMP module will append `nosto-cmp-mapping` block which contains a map of category urls and hashes. 
This will be used as a mechanism to generate different cached versions based on segments. 

The client script will append a query parameter to the category url like `example.com/category?key=xyz`. Each segment will have its own key value and the value is defined by Nosto. Magento will generate a new cached version of the category page based on the url parameters when a user lands on the page (in case the cached version is missing for the specific semgent).
Users within the same segment will be served the same cached segment sorting. 

## Category Mapping Cache

Building the category map might become a slow operation. Our module introduces a dedicated cache to store the mapping. 
The cache is named `nosto_category_mapping_cache`. 

To enable the cache just run the magento command:
```commandline
bin/magento cache:enable nosto_category_mapping_cache
```
