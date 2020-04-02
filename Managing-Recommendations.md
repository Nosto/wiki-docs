The module adds a default set of recommendation elements to the shop, that can be moved/removed as you see fit. The following recommendation slots are added:

| Page     | Rec Slot               | Description                               | Hook                     | Custom Hook            |
|----------|------------------------|-------------------------------------------|--------------------------|------------------------|
| Front    | `frontpage-nosto-1`    | Personalized Recommendations              | `HOOK_HOME`              |                        |
| Front    | `frontpage-nosto-2`    | Best Sellers and Trending Products        | `HOOK_HOME`              |                        |
| Front    | `frontpage-nosto-3`    | Browsing History                          | `HOOK_HOME`              |                        |
| Front    | `frontpage-nosto-4`    | Browsing History related Recommendations  | `HOOK_HOME`              |                        |
| Category | `nosto-page-category1` | Best Sellers and Trending Products        | `HOOK_TOP`               | `HOOK_CATEGORY_TOP`    |
| Category | `nosto-page-category2` | Browsing History                          | `HOOK_TOP`               | `HOOK_CATEGORY_FOOTER` |
| Product  | `nosto-page-product1`  | Cross-Selling and Up-Selling              | `HOOK_PRODUCT_FOOTER`    |                        |
| Product  | `nosto-page-product2`  | Cross-Selling and Up-Selling              | `HOOK_PRODUCT_FOOTER`    |                        |
| Product  | `nosto-page-product3`  | Cross-Selling and Up-Selling              | `HOOK_PRODUCT_FOOTER`    |                        |
| Cart     | `nosto-page-cart1`     | Shopping Cart Recommendations             | `HOOK_SHOPPING_CART`     |                        |
| Cart     | `nosto-page-cart2`     | Personalized Recommendations              | `HOOK_SHOPPING_CART`     |                        |
| Cart     | `nosto-page-cart3`     | Best Sellers and Trending Products        | `HOOK_SHOPPING_CART`     |                        |
| Search   | `nosto-page-search1`   | Search and Visit related Recommendations  | `HOOK_TOP`               | `HOOK_SEARCH_TOP`      |
| Search   | `nosto-page-search2`   | Browsing History                          | `HOOK_TOP`               | `HOOK_SEARCH_FOOTER`   |
| Order    | `thankyou-nosto-1`     | Order Related Recommendations             | `HOOK_ORDER_CONFIRMATION`|                        |


## Custom Hooks:

Prestashop does not provide it's own top and footer hooks for the category page. In order to overcome this limitation we have provided a workaround:

1. Autoslots: In order to compensate for the missing hooks, there is a custom JS that finds a DIV element with the class `center-column` and appends and prepends the respective slots before and after this DIV. This DIV element — present on most Prestashop themes — is the container for the main content block.
2. Custom Hooks: We have created four custom hooks that can be explicitly placed on the category and search pages for precise positioning of the top and footer recommendation slots of the category and search pages.

### Category Page:

Edit the search template and place the following snippet precisely where you you want the top and footer recommendations to appear.

| Page   |                                                                                          |
|--------|------------------------------------------------------------------------------------------|
| Top    | `{if isset($HOOK_CATEGORY_TOP) && $HOOK_CATEGORY_TOP}{$HOOK_CATEGORY_TOP}{/if}`          |
| Footer | `{if isset($HOOK_CATEGORY_FOOTER) && $HOOK_CATEGORY_FOOTER}{$HOOK_CATEGORY_FOOTER}{/if}` |

### Search Page:

Edit the search template and place the following snippet precisely where you you want the top and footer recommendations to appear.

| Page   |                                                                                          |
|--------|------------------------------------------------------------------------------------------|
| Top    | `{if isset($HOOK_SEARCH_TOP) && $HOOK_SEARCH_TOP}{$HOOK_SEARCH_TOP}{/if}`                |
| Footer | `{if isset($HOOK_SEARCH_FOOTER) && $HOOK_SEARCH_FOOTER}{$HOOK_SEARCH_FOOTER}{/if}`       |


## Moving recommendations

In order to move the position of a recommendation element, you have the following options:

### Re-position the PrestaShop hook in your theme

The recommendations are all added via PrestaShop hooks, which means that the elements are shown in the places where the hooks are called in the theme files. The benefit of this is that you have full control over the content displayed via this hook, i.e. you can choose the order of the modules that add content using the hook (if there are more than one) or you can choose to disable a specific module content altogether.

By moving the hook call in the template file, which will look something like `{if isset($HOOK_PRODUCT_FOOTER) && $HOOK_PRODUCT_FOOTER}{$HOOK_PRODUCT_FOOTER}{/if}`, you can control where on the page the content will appear. This may not be an option if the hook in question outputs content for other than the Nosto module, as you may want to keep the other content in the place it is in, and just move the Nosto recommendations. In this case you can add the recommendation element manually as described below.

### Add the recommendation element placeholder manually in the theme

Adding the recommendation element manually to the theme is as simple as inserting a html `<div>` element in your theme file. But before that, you want to disable the hook for the module, so that it does not output the recommendation element in the default place as well. This is done in your PrestaShop backend on the `Modules -> Positions` page. Locate the name of the hook you want to remove Nosto from, and choose `Unhook` from dropdown menu on the right hand side for the module.

Now open up your theme file and add the html for the recommendation element. The element html that is used by default is:

* The home page

    \<div class="nosto_element" id="frontpage-nosto-1"\>\</div\>  
    \<div class="nosto_element" id="frontpage-nosto-2"\>\</div\>  
    \<div class="nosto_element" id="frontpage-nosto-3"\>\</div\>  
    \<div class="nosto_element" id="frontpage-nosto-4"\>\</div\>

* The category pages

  \<div class="nosto_element" id="nosto-page-category1"\>\</div\>  
  \<div class="nosto_element" id="nosto-page-category2"\>\</div\>  

* The product pages

    \<div class="nosto_element" id="nosto-page-product1"\>\</div\>  
    \<div class="nosto_element" id="nosto-page-product2"\>\</div\>  
    \<div class="nosto_element" id="nosto-page-product3"\>\</div\>  

* The search result page

    \<div class="nosto_element" id="nosto-page-search1"\>\</div\>  
    \<div class="nosto_element" id="nosto-page-search2"\>\</div\>  

* The shopping cart page

    \<div class="nosto_element" id="nosto-page-cart1"\>\</div\>  
    \<div class="nosto_element" id="nosto-page-cart3"\>\</div\>  
    \<div class="nosto_element" id="nosto-page-cart2"\>\</div\>  

You can place these elements anywhere you like on the page, and Nosto will populate them with product recommendations automatically. Note that the above elements are only the default ones added by the module, and you can add you're own elements in addition or replacing the default ones.

## Removing recommendations

If you wish to remove one of the default recommendation elements you can easily do so by going to the module configuration page, and for the correct account toggle of the visibility of the recommendation. These settings are found on the `Recommendations` page of the account management.

Recommendations that are not visible will not be populated with content by Nosto. The recommendation element placeholder will still be added to the site, but it will remain empty and hidden to the users.

## Adding new recommendations

You can add your own customised recommendation elements to your shop by following the same instructions as in the "Add the recommendation element placeholder manually in the theme" section.

Note that the element IDs need to match defined element's in Nosto. These you need to create and configure for you're Nosto account by logging in to the [Nosto backend](https://my.nosto.com).