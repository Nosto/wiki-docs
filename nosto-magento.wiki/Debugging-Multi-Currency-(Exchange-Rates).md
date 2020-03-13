When using multi-currency (using exchange rates), please check the following:

## In Magento

1. Ensure that the `nosto_variation` tagging is present on all pages. This element denotes the current currency in the context. Changing the currency of the store should change this to denote the correct currency.

2. Ensure that the product tagging has the `variation_id` tagging. This element denotes the base currency of the website (not store-view). 



## In Nosto

1. Ensure that at least on of each of the following API tokens are granted:
    - API_SSO
    - API_RATES
    - API_PRODUCTS
    - API_SETTINGS

    _In the event that either the API_RATES or the API_SETTINGS is missing, remove the Nosto account and initiate the OAuth (connect-with-nosto) again. This will issue a fresh set of API tokens._

    [https://my.nosto.com/admin/**merchant**/account/settings/tokens](https://my.nosto.com/admin/<merchant>/account/settings/tokens)

2. Ensure that the products in the Nosto admin have the variation identifier set.
    
    [https://my.nosto.com/admin/**merchant**/campaigns/products](https://my.nosto.com/admin/<merchant>/campaigns/products)

    _If the product is missing the variation identifier, this could be due to the fact the merchant migrated from a non-multi-currency extension to a multi-currency extension. Flushing all products (via support) and initiating the OAuth (connect-with-nosto) again in that particular order will resolve the issue._

3. Ensure that the currency formats for each of the website currencies are set. While this will not cause any issues, Nosto will use the default currency formats for that currency.

    _This can be fixed by going to Nosto extension configuration in Magento (Configuration >> System >> Services >> Nosto) and clicking "Save Config". We do not recommend for this to be fixed manually as currency formats can vary widely from installation to installation._

4. Ensure that the switch "Use exchange rates" is active. This enables or disables the exchange rates mechanism.

    [https://my.nosto.com/admin/**merchant**/account/settings/multicurrency](https://my.nosto.com/admin/<merchant>/account/settings/multicurrency)

    _This can be fixed either manually or by going to Nosto extension configuration in Magento (Configuration >> System >> Services >> Nosto) and clicking "Save Config"._

5. Ensure that the field "Variation ID of the primary currency" is configured for the merchant. This value is the base currency code for the website.

    [https://my.nosto.com/admin/**merchant**/account/settings/multicurrency](https://my.nosto.com/admin/merchant/account/settings/multicurrency)

    _This can be fixed either manually or by going to Nosto extension configuration in Magento (Configuration >> System >> Services >> Nosto) and clicking "Save Config"._