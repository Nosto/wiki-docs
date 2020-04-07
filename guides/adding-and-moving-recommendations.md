# Adding & Moving Recommendations

All recommendations are added through the PrestaShop hook system, which means that they are dependent on the existence and positioning of the hooks in the theme.

The module adds a default set of recommendation elements to the shop, that can be moved/removed as you see fit. The following recommendation slots are added:

| Page | Rec Slot | Description | Hook | Custom Hook |
| :--- | :--- | :--- | :--- | :--- |
| Front | `frontpage-nosto-1` | Personalized Recommendations | `HOOK_HOME` |  |
| Front | `frontpage-nosto-2` | Best Sellers and Trending Products | `HOOK_HOME` |  |
| Front | `frontpage-nosto-3` | Browsing History | `HOOK_HOME` |  |
| Front | `frontpage-nosto-4` | Browsing History related Recommendations | `HOOK_HOME` |  |
| Category | `nosto-page-category1` | Best Sellers and Trending Products | `HOOK_TOP` | `HOOK_CATEGORY_TOP` |
| Category | `nosto-page-category2` | Browsing History | `HOOK_TOP` | `HOOK_CATEGORY_FOOTER` |
| Product | `nosto-page-product1` | Cross-Selling and Up-Selling | `HOOK_PRODUCT_FOOTER` |  |
| Product | `nosto-page-product2` | Cross-Selling and Up-Selling | `HOOK_PRODUCT_FOOTER` |  |
| Product | `nosto-page-product3` | Cross-Selling and Up-Selling | `HOOK_PRODUCT_FOOTER` |  |
| Cart | `nosto-page-cart1` | Shopping Cart Recommendations | `HOOK_SHOPPING_CART` |  |
| Cart | `nosto-page-cart2` | Personalized Recommendations | `HOOK_SHOPPING_CART` |  |
| Cart | `nosto-page-cart3` | Best Sellers and Trending Products | `HOOK_SHOPPING_CART` |  |
| Search | `nosto-page-search1` | Search and Visit related Recommendations | `HOOK_TOP` | `HOOK_SEARCH_TOP` |
| Search | `nosto-page-search2` | Browsing History | `HOOK_TOP` | `HOOK_SEARCH_FOOTER` |
| Order | `thankyou-nosto-1` | Order Related Recommendations | `HOOK_ORDER_CONFIRMATION` |  |
| Order | `thankyou-nosto-2` |  | `HOOK_ORDER_CONFIRMATION` |  |
| 404 | `notfound-nosto-1` | Browsing History related Recommendations | `HOOK_TOP` |  |
| 404 | `notfound-nosto-2` | Best Sellers and Trending Products | `HOOK_TOP` |  |
| 404 | `notfound-nosto-3` | Landing Page Recommendations | `HOOK_TOP` |  |

### Custom Hooks:

Prestashop does not provide it's own top and footer hooks for the category page. In order to overcome this limitation we have provided a workaround:

1. Autoslots: In order to compensate for the missing hooks, there is a custom JS that finds a DIV element with the class `center-column` and appends and prepends the respective slots before and after this DIV. This DIV element — present on most Prestashop themes — is the container for the main content block.
2. Custom Hooks: We have created four custom hooks that can be explicitly placed on the category and search pages for precise positioning of the top and footer recommendation slots of the category and search pages.

#### Category Page:

Edit the search template and place the following snippet precisely where you you want the top and footer recommendations to appear.

| Page |  |
| :--- | :--- |
| Top | `{if isset($HOOK_CATEGORY_TOP) && $HOOK_CATEGORY_TOP}{$HOOK_CATEGORY_TOP}{/if}` |
| Footer | `{if isset($HOOK_CATEGORY_FOOTER) && $HOOK_CATEGORY_FOOTER}{$HOOK_CATEGORY_FOOTER}{/if}` |

#### Search Page:

Edit the search template and place the following snippet precisely where you you want the top and footer recommendations to appear.

| Page |  |
| :--- | :--- |
| Top | `{if isset($HOOK_SEARCH_TOP) && $HOOK_SEARCH_TOP}{$HOOK_SEARCH_TOP}{/if}` |
| Footer | `{if isset($HOOK_SEARCH_FOOTER) && $HOOK_SEARCH_FOOTER}{$HOOK_SEARCH_FOOTER}{/if}` |

## Change position of recommendation elements

In order to change the position of any recommendation element added by Nosto, you can either move the PrestaShop hook position in your theme or unlink the Nosto module from the hook and add the elements directly into your theme. Please refer to the PrestaShop documentation on how to re-position hooks.

During the module installation, some new hooks are also created. These are:

* `displayCategoryTop`
* `displayCategoryFooter`
* `displaySearchTop`
* `displaySearchFooter`

These can be used to position the recommendations on the product category and search result pages, as PrestaShop does not include any hooks out-of-the-box for these pages. The module will automatically add the recommendations without these hooks as well, but for more precise positioning we recommend to include them in your theme. This is as easy as adding a line like `{hook h='displayCategoryTop'}` to your theme layout file.

## Removing recommendations elements

If you wish to remove one of the default recommendation elements you can easily do so by going to the module configuration page, and for the correct account toggle of the visibility of the recommendation. These settings are found on the Recommendations page of the account management.

Recommendations that are not visible will not be populated with content by Nosto. The recommendation element placeholder will still be added to the site, but it will remain empty and hidden to the users.

## Adding new recommendation elements

The easiest way to add your own recommendation elements is to simply add the placeholder "div" in your theme layout, e.g. `<div class="nosto_element" id="{id-of-your-choice}"></div>`. Note that you need to register this new element in your [Nosto account settings](https://my.nosto.com/), so that Nosto can start using it.

