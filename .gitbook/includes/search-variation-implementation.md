The search/category merchandising (universal) API supports specifying the variation ID in requests
using the `variationId` property of the [products object](https://search.nosto.com/v1/graphql?ref=InputSearchProducts).

If specified, the selected variation's specific properties automatically replace the corresponding values in the top-level product.
All features (e.g., merchandising rules, facets, filters, sorting) work with the selected variation's values automatically when the variation ID is provided.

For Nosto code editor integrations, please refer to a
[simplified version of this process](../../implement-search/implement-search-using-code-editor/implementing-search-page#multi-currency).