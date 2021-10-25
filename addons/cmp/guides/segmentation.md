# Segmentation in Category Merchandising

Merchandising supports segmentation. The segments can be [configured in Nosto](https://help.nosto.com/en/articles/3067796-segmentation-targeting-options).

The segmentation logic is handled by the Nosto client script. Nosto's CMP module will append `nosto-cmp-mapping` block which contains a map of category urls. 
The client script will append a query parameter to all matching urls in the DOM. The query parameter will be based on the 
segment that the customer falls into. With `full_page cache` enabled Magento will generate a cached version of the category 
page per segment.