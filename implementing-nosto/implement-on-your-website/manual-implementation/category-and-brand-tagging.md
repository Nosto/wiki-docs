# Adding the Category/Brand Tagging

Nosto utilizes meta tags to track what category or brand a certain visitor is viewing or what page type the currently viewed page is. These values are then used for dynamic filtering for categories and brands applied through the Nosto admin UI or exposure of certain pop-up campaigns for page types.

The category tagging should be exposed whenever a user is viewing a certain category.

```js
nostojs(api => {
  api.setTaggingProvider("pageType", "category")
  api.setTaggingProvider("categories", ["/Mens/Jackets/Ski Jackets"]) 
})
```

or via DOM tagging

```markup
<div class="nosto_page_type" style="display:none" translate="no">category</div>
<div class="nosto_category" style="display:none" translate="no">/Mens/Jackets/Ski Jackets</div>
```

The brand tagging should be exposed whenever a user is viewing a certain brand or vendor.

```js
nostojs(api => {
  api.setTaggingProvider("pageType", "category")
  api.setTaggingProvider("brands", ["Acme"]) 
})
```

or via DOM tagging

```markup
<div class="nosto_page_type" style="display:none" translate="no">category</div>
<div class="nosto_brand" style="display:none" translate="no">Acme</div>
```

### Tagging the categories

Categories must always be delimited by a slash. For example, `/Home/Accessories` is a valid category while `Home > Accessories` is not.

### Faceting by other attributes

With Nosto you can also expose other attributes that should be used for category/brand page filtering. For example when a user clicks on a certain color, only products with that certain color attribute should be exposed by both the category list, and Nosto Onsite Recommendations. Available values correspond to custom fields tagged as part of the [Product Tagging](product-tagging/default-product-tagging.md).

```js
nostojs(api => {
  api.setTaggingProvider("tags", ["color: Red", "gender: Men"])
})
```

or via DOM tagging

```markup
<span class="nosto_tag" style="display:none" translate="no">color: Red</span>
<span class="nosto_tag" style="display:none" translate="no">gender: Men</span>
```

### Tagging the current page type

Page type tagging should be exposed whenever a user is interacting with a page so Nosto understands what kind of page this is.

```js
nostojs(api => {
  api.setTaggingProvider("pageType", "category")    
})
```

or via DOM tagging

```markup
<div class="nosto_page_type" style="display:none" translate="no">category</div>
```

Page type is optional and used mainly for triggering popups and also to understand what kind of page the user is currently interacting with. The page type must always be lowercase and the accepted values for page type are: `front`, `category`, `produc`, `cart`, `order`, `search` and `notfound`.

## Troubleshooting

Once included on all pages, you can review if the site is transmitting data using the Nosto Debug Toolbar. If you can see order contents being picked up under "Tagging" → "Category" then the category and page type tagging are correctly set up in the source code.

![Nosto debug category ](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-debug-toolbar-category.png)

### Translate attribute

The translate attribute is a [HTML5 standard attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/translate) which specifies whether the value of the element and it's `Text` node children should be translated. If your tagging elements are being translated by e.g. Google Translator then this is the way to opt out elements being translated by Google and possibly other vendors.