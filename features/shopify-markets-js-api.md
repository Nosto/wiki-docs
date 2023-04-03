# Shopify Markets Workaround

{% hint style="info" %} Please note that this is a temporary workaround for loading recommendations based on Shopify Market until we release a complete end to end support. This workaround avoids any manual setup to be done on merchant's store. Instead, it involves minor changes to the existing recommendation templates.
{% endhint %}

## JS API to the rescue

### Drawbacks of current approach
In order to workaround product pricing for Shopify Markets, the existing approach follows the changes outlined [here](/features/multi-currency-support.md#fetch-product-prices-in-presentment-currency-legacy). This involves updating Shopify theme liquid template manually in order to enable Shopify Markets. The approach is also limited to loading product prices for the market. 

### The JS API
In order to overcome the drawbacks of the current approach, we introduce a new JS API `migrateToShopifyMarket`. Using this API, you can load recommendations with following details updated for the current market, automatically.
1. Product URL - updates Product URL for every product in the recommendation to match the URL of the store's market. At the same time, the URL will retain all the attribution parameters from the original URL
2. Product Title - updates Product Title, for every product in the recommendation, to reflect the language of the current market
3. Product Description - updates Product description, for every product in the recommendation, to reflect the language of the current market
4. Product Category - updates Product category, for every product in the recommendation, to reflect the language of the current market
5. Product Variant Options - For those recommendations involving chosing variants, this API translates all the variant options and title to reflect the language of the current market

#### Usage
The API takes an Object `RecProductElementSelectors` as a single parameter. The following table lists all the allowed mandatory and optional parameters of `RecProductElementSelectors` Object

| Parameter Name | Required/Optional | Type | Description |
| :-------------------: | :---------------------: | :----------: | :-------------- |
| productSectionElement | Required | String | CSS classname/Id selector of the `div` or an equivalent wrapper element that encapsulates all the product details and the variant options  |
| productUrlElement |  Required  | String |CSS classname selector of all <a> elements/Id of the <a> element hosting the product URL |
| productTitleElement | Required | String | CSS classname selecror/Id of the element hosting the product title |
| productHandleAttribute | Required | String | Attribute name for fetching the product handle. This value can be fetched in recommendation template using `$!product.lastPathOfProductUrl`. This attribute has to be part of the `div` or an equivalent wrapper element hosting the `productSectionElement` |
| priceElement | Required | String | CSS classname selector of all elements or Id of the element displaying product `price` value. This parameter is required and is used to display the actual product price |
| listPriceElement | Optional | String | CSS classname selector of all elements or Id of the element displaying product `compare_at_price` value. This is used only when the product has a discounted price and the recommendation displays both old & new price  |
| listPriceElement | Optional | String | CSS classname selector of all elements or Id of the element displaying product `compare_at_price` value. This is used only when the product has a discounted price and the recommendation displays both old & new price  |
| defaultVariantIdAttribute | Optional | String | This parameter is used only when the recommendation doesn't contain variant options. When not provided, first variant of product is used. It is an Attribute element and is used for determining the product pricing for the market. It should be part of the `div` or an equivalent wrapper element hosting the `productSectionElement` |
| descriptionElement | Optional | String | CSS classname selector of all elements or Id of the element displaying product description. Needed only when the recommendation displays product description |
| categoryNameElement | Optional | String | CSS classname selector of all elements or Id of the element displaying product category. Needed only when the recommendation displays product category |
| totalVariantOptions | Optional | Number | Total variant option levels displayed in the recommendation. Here variant level means, the selection of one variant value loads the immediate next variant and so on. More on this in the below sections. When not provided, actual product variant count is used. This value can be below the total actual product variants but not above that value. Currently, we support only a maxium of 5 variant option levels |
| variantSelector | Optional | Object/VariantOptionSelector | Needed only when recommendations display variant options. More on this in the next section |

The following table lists all the allowed mandatory and optional parameters of `VariantOptionSelector` Object. This parameter is required only when the recommendation displays variant options.

| Parameter Name | Required/Optional | Type | Description |
| :-------------------: | :---------------------: | :----------: | :-------------- |
| titleElement | Optional | String |  CSS classname selector/Id of the element hosting the variant option title. Either this or `optionElement` selector must be provided. Both are not mandatory. Title element won't be updated when this option is ignored |
| optionElement | Optional | String |  CSS classname selector of all the elements hosting the variant option values. This selector should be in-depth such that it directly points to the element hosting the option value. Either this or `titleElement` selector must be provided. Both are not mandatory. Option elements won't be updated when this option is ignored |
| selectedElement | Optional | String | Must be provided when `optionElement` selector is provided. CSS selector/id of the element selected in the variant option. In case of multiple variant options, It's required to use the same selector across all the variant options to avoid any issues.  | 
| position | Optional | Number | Must be provided when `optionElement` selector is provided. Numeric value of the order number/position of the variant option as declared in the Shopify admin product page. It's not required to display the elements, in the same order, in the recommendations, but it's requred to supply the appropriate position number and it should match the order in the Shopify admin |
| dependentVariantSelector | Optional | Object/VariantOptionSelector | Selectors configuration for sub-level variants that changes based on the value selected in this primary variant. A maximum of 5 sub-level variants can be supported |
