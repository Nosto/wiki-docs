# FAQ

{% hint style="info" %}
Additionally, please refer to [our help pages](https://help.nosto.com/en/collections/3914250-search) for further useful information!
{% endhint %}

## Search returns up to 10000 documents

Due to performance optimization, the search function will calculate the total count up to 10,000. In this case the search page should display a count of `10,000+` to indicate that more than 10,000 products were found.

Filters and sorting operations are executed on all found products, even if there are more than this limit. Therefore, it's still possible to find other products if you filter or sort them. This should not affect the user experience in any way because it's unlikely that someone would actually view more than 10,000 products with a single search.

## Products in category listings are in a different order as soon as I deliver results from Nosto API

As soon as you use the results delivered by Nosto API, you will see that the listings in categories have a different order than listings delivered by your native shop-system, even if you didn't set up any merchandising rules.
The products are mainly delivered as indexed during the data sync, but there is no defined behaviour for this.
We recommend to always set up at least one global rule before going live with Nosto category merchandising to create product listings matching your business strategies.

