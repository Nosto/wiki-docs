This document outlines adding the Nosto tagging into SiteGenesis store front. Below you can find a list of template files and a code snippet what to add to a specific template file. The paths for template files given below are based on SiteGenesis so you must make sure they to add the code snippets in their relevant cartridge.

### htmlhead.isml
Adds Nosto include script to all pages (html head). 

Path: `SiteGenesis_core/cartridge/templates/default/components/header/htmlhead.isml`

```
<iscomment>NOSTO Script Tag</iscomment>
<isinclude template="nostoHeaderScript"/>
``` 
![image](https://user-images.githubusercontent.com/15191701/53884422-385b0400-4024-11e9-91d6-ea9b06ca0a0d.png)

### pt_storefront.isml
Adds page type tagging to the home page / front page of a store. 

Path: `SiteGenesis_core/cartridge/templates/default/content/home/pt_storefront.isml`

```
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getHomePageTypeTag()}" encoding="off"/>
``` 

![image](https://user-images.githubusercontent.com/15191701/53884650-c636ef00-4024-11e9-9d15-83bf03982b3a.png)

### pt_cart.isml
Adds page type tagging to the cart page. 

Path: `SiteGenesis_core/cartridge/templates/default/checkout/cart/pt_cart.isml`

``` 
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCartPageTypeTag()}" encoding="off"/>
``` 
![image](https://user-images.githubusercontent.com/15191701/53884769-fed6c880-4024-11e9-971f-081fe14b5c89.png)

### footer.isml
Adds cart tagging to all pages. 

Path: `SiteGenesis_core/cartridge/templates/default/components/footer/footer.isml`

```
<isscript>
	var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCustomerTags(pdict.currentCustomer)}" encoding="off"/>
<isprint value="${nostoHelper.getCartTag()}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53884907-3b0a2900-4025-11e9-933b-3e4190d54eb9.png)

### pt_productdetails.isml
Adds product tagging to product detail pages. 

Path: `SiteGenesis_core/cartridge/templates/default/product/pt_productdetails.isml`

``` 
<isscript>
	var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getProductTags(pdict.Product)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53884977-5e34d880-4025-11e9-90c7-42e43fb4d446.png)

### pt_categorylanding.isml
Adds category tagging into category pages. 

Path: `SiteGenesis_core/cartridge/templates/default/search/pt_categorylanding.isml` 

``` 
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCategoryTags(pdict.CurrentHttpParameterMap.cgid)}" encoding="off"/>
``` 

![image](https://user-images.githubusercontent.com/15191701/53885184-c4216000-4025-11e9-870b-5d3ab29c8b28.png)

### pt_productsearchresult.isml 
Adds category tagging to search results page.  

Path: `SiteGenesis_core/cartridge/templates/default/search/pt_productsearchresult.isml`

``` 
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getCategoryTags(pdict.CurrentHttpParameterMap.cgid)}" encoding="off"/>
```

![image](https://user-images.githubusercontent.com/15191701/53885253-ec10c380-4025-11e9-95a8-9bb6a9945d12.png)

### pt_orderconfirmation.isml
Adds order tagging to order confirmation page. 

Path: `SiteGenesis_core/cartridge/templates/default/checkout/pt_orderconfirmation.isml`

```
<isscript>
    var nostoHelper = require('int_nosto/cartridge/scripts/helpers/nostoHelper').getNostoHelper();
</isscript>
<isprint value="${nostoHelper.getOrderTags(pdict.Order)}" encoding="off"/>
``` 

![image](https://user-images.githubusercontent.com/15191701/53885324-15315400-4026-11e9-9e79-3c3fabbe1086.png)
