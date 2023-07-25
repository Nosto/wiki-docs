# FAQ

{% hint style="info" %}
Additionally, please refer to [our help pages](https://help.nosto.com/en/collections/3914250-search) for further useful information!
{% endhint %}

## Search returns up to 10000 documents

Due to performance optimization, the search function will calculate the total count up to 10,000. In this case the search page should display a count of `10,000+` to indicate that more than 10,000 products were found.

Filters and sorting operations are executed on all found products, even if there are more than this limit. Therefore, it's still possible to find other products if you filter or sort them. This should not affect the user experience in any way because it's unlikely that someone would actually view more than 10,000 products with a single search.

