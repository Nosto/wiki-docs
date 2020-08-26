# Varnish & Full-Page Caches

Nosto injects the details of the currently logged in customer, the contents of the current shopping cart, and the currently active currency. It does so on all pages. Since these data for these are session dependant, they should never be cached. As a solution for this, Nosto for Shopware supports hole-punching these sections via it's built-in caching support.

{% hint style="danger" %}
You must ensure that no sensitive personal-data is cached. In the event that personal-data is cached, it will leak across sessions and in cases, may even get indexed by search engine crawler.
{% endhint %}

Nosto for Shopware works out of the box with [Shopware's HTTP cache.](https://developers.shopware.com/developers-guide/http-cache/) Nosto does this by leveraging [Shopware's support for Action Tags](https://developers.shopware.com/blog/2016/07/11/on-action-tags/). Action Tags automatically hole-punch certain parts of the page and when used with Varnish, it autogenerates the necessery Edge Side Includes \(ESI\). 

{% hint style="warning" %}
This feature is only supported from version **2.4.8** onwards. If you are running an older version, we recommend you upgrade immediately.
{% endhint %}

Nosto adds query parameters to the product URLs for tracking purposes. You may encounter a cache-miss when query parameters are appended to the URL. We recommend [modifying your VCL definition to automatically strip out the query parameters added by Nosto](https://help.nosto.com/manuals/how-can-i-exclude-the-nosto-query-parameter-from-my-varnish-vcl-rules).

