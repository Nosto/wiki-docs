Nosto for Magento works out of the box with the [Varnish support in Magento 2](http://devdocs.magento.com/guides/v2.3/config-guide/varnish/config-varnish.html).

Nosto adds query parameters to the product URLs for tracking purposes. You may encounter a cache-miss when query parameters are appended to the URL. We recommend [modifying your VCL definition to automatically strip out the query parameters added by Nosto](https://help.nosto.com/manuals/how-can-i-exclude-the-nosto-query-parameter-from-my-varnish-vcl-rules).
