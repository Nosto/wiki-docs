# Tagging your page types

The page-type tagging enables Nosto to trigger actions, such as showing popups, depending upon a page type. Tagging the page types is optional but without the page-type tagging, you will not be able to avail the use of page type based triggers.

Here is a list of all the valid page types:

* The home page of your store should be tagged as `front`.
* All category pages should be tagged as `category`.
* All product pages should be tagged as `product`.
* The shopping cart page should be tagged as `cart` .
* The checkout page, where order information is filled, should be tagged as `checkout`.
* The order confirmation page should be tagged as `order`.
* The search results page should be tagged as `search`.
* All no-found pages should be tagged as `notfound`.
* Other pages should be tagged as `other`.

```markup
 <div class="nosto_page_type" style="display:none">product</div>
```

