# Configuring

The extension can be configured by navigating to Stores &gt; Configuration &gt; Services &gt; Nosto Category Merchandising.

## Check the tokens

The APPS token should be present to enable the usage of category merchandising. If it is missing, please reconnect the account by navigating to Marketing -&gt; Nosto -&gt; Nosto Dashboard -&gt; Settings and press on Reconnect.

## Enabling/Disabling CMP

Navigate to `Stores -> Settings -> Configuration -> Service -> Nosto Category Merchandising` and select `Yes` in the dropdown

![](../../.gitbook/assets/cmp1.png)

## Map all categories

Nosto makes use of a map of categories for caching and tracking reasons. By default the category list is limited 
to only categories that are used in the storefront. In case you are using categories that are not visible, you need 
to enable mapping all categories

![image](https://user-images.githubusercontent.com/44775916/101625474-1fb32580-3a24-11eb-8854-77de887a87cc.png)

## Products limit per page

The module limits maximum products fetched from Nosto to 250 by default. 
This is related to the limit set for our internal cmp query. 

In case you have enabled the `Show all products` feature on our catalog page, 
you can increase the limit to what fits your best needs. 
In order to increase it navigate to `Stores > Settings > Configuration > Services > Nosto Category Merchandising` 
and change the value for `Max product limit` 

![image](https://user-images.githubusercontent.com/44775916/97557928-1cb82480-19e4-11eb-9b6e-1f0170600ef7.png)

#### Warning
Increasing the limit can affect the catalog page load time. The module will make multiple calls
to iterate through the number of pages that match the new maximum limit of products.


