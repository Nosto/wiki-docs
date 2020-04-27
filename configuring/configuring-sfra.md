# On SFRA

This document outlines adding the Nosto tagging using Storefront Reference Architecture \(SFRA\). You can find an example SFRA implementation [here](https://github.com/Nosto/nosto-sfcc/tree/master/cartridges/int_nosto_sfra/cartridge).

Path for files below are based on SFRA so you must make sure you are adding the code snippets into the relevant cartridge. Sample files below are present in [int\_nosto\_sfra](https://github.com/Nosto/nosto-sfcc/tree/master/cartridges/int_nosto_sfra/cartridge) cartridge for comparison purpose.

## htmlHead.isml

Adds the Nosto include script to all pages \(html head\).

Path: `app_storefront_base/cartridge/templates/default/common/htmlHead.isml`

```text
<iscomment>NOSTO Script Tag</iscomment>
<isinclude template="nostoHeaderScript"/>
```

![image](https://user-images.githubusercontent.com/15191701/53886996-fa60de80-4029-11e9-960d-ee5d5ef839cf.png)

## homePage.isml

Adds page type tagging to the home page / front page of a store.

Path: `app_storefront_base/cartridge/templates/default/home/homePage.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getHomePageTypeTag()}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887104-3dbb4d00-402a-11e9-984c-68d6536645fd.png)

## cart.isml

Adds page type tagging to the cart page.

Path: `app_storefront_base/cartridge/templates/default/cart/cart.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCartPageTypeTag()}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887160-5c214880-402a-11e9-81a1-a7a2822f93fc.png)

## pageFooter.isml

Adds cart tagging to all pages.

Path: `app_storefront_base/cartridge/templates/default/components/footer/pageFooter.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCartTag()}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887203-76f3bd00-402a-11e9-9508-78e0c1c16da1.png)

## productDetails.isml

Adds product tagging to product detail pages.

Path: \`app\_storefront\_base/cartridge/templates/default/product/productDetails.isml\`\`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getProductTags(pdict.product.id)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887252-9559b880-402a-11e9-9060-e5bc1c83195c.png)

## bundleDetails.isml

Adds product tagging to product bundle pages.

Path: `app_storefront_base/cartridge/templates/default/product/bundleDetails.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getProductTags(pdict.product.id)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887312-b0c4c380-402a-11e9-8404-267e2a9f0a06.png)

## setDetails.isml

Adds product tagging to product set pages.

Path: `app_storefront_base/cartridge/templates/default/product/setDetails.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getProductTags(pdict.product.id)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887359-c33efd00-402a-11e9-8a9c-8b3263056f62.png)

## searchResults.isml

Adds category tagging to search results page.

Path: `app_storefront_base/cartridge/templates/default/search/searchResults.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCategoryTags(pdict.productSearch.category.id)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887413-e36ebc00-402a-11e9-9d58-48807752162f.png)

## catLanding.isml

Adds category tagging into category pages.

Path: `app_storefront_base/cartridge/templates/default/rendering/category/catLanding.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCategoryTags(pdict.productSearch.category.id)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887445-faada980-402a-11e9-871b-cf6d06f036be.png)

## confirmation.isml

Adds order tagging to order confirmation page.

Path: `app_storefront_base/cartridge/templates/default/checkout/confirmation/confirmation.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getOrderTags(pdict.order.orderNumber)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887510-1ca72c00-402b-11e9-8718-e0b318ae4bb5.png)

## page.isml

Adds customer tagging to all pages.

Path: `app_storefront_base/cartridge/templates/default/common/layout/page.isml`

```text
<isscript>
   var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCustomerTags()}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887548-347eb000-402b-11e9-94d6-8e6c0182877c.png)

## checkout.isml

Adds customer tagging to checkout pages.

Path: `app_storefront_base/cartridge/templates/default/common/layout/checkout.isml`

```text
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCustomerTags()}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53887641-5bd57d00-402b-11e9-8179-eaef5edc4ff2.png)

