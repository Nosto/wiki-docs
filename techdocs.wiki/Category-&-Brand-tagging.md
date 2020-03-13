Nosto utilizes meta tags to track what category or brand a certain visitor is viewing or what page type the currently viewed page is. These values are then used for dynamic filtering for categories and brands applied through the Nosto admin UI or exposure of certain pop-up campaigns for page types.  

The category tagging should be exposed whenever a user is viewing a certain category. 

```html
<div class="nosto_page_type" style="display:none">category</div>
<div class="nosto_category" style="display:none">/Mens/Jackets/Ski Jackets</div>
```

The brand tagging should be exposed whenever a user is viewing a certain brand or vendor. 

```html
<div class="nosto_page_type" style="display:none">category</div>
<div class="nosto_category" style="display:none">Acme</div>
```

### Tagging the categories

Categories must always be delimited by a slash. For example, `/Home/Accessories` is a valid category while `Home > Accessories` is not.

### Faceting by other attributes

With Nosto you can also expose other attributes that should be used for category/brand page filtering. For example when a user clicks on a certain color, only products with that certain color attribute should be exposed by both the category list, and Nosto Onsite Recommendations. Available values correspond to custom fields tagged as part of the [Product Tagging](https://github.com/Nosto/techdocs/wiki/Basic:-Default-Product-Tagging). 
```html
<span class="nosto_tag" style="display:none">color: Red</span>
<span class="nosto_tag" style="display:none">gender: Mens</span>
```

### Tagging the current page type

Page type tagging should be exposed whenever a user is interacting with a page so Nosto understands what kind of page this is. 
```html
<div class="nosto_page_type" style="display:none">category</div>
```

Page type is optional and used mainly for triggering popups and also to understand what kind of page the user is currently interacting with. The page type must always be lowercase and the accepted values for page type are: front, category, product, cart, order, search, notfound

## Troubleshooting

Once included on all pages, you can review if the site is transmitting data using the Nosto Debug Toolbar. If you can see order contents being picked up under "Tagging" → "Category" then the category and page type tagging are correctly set up in the source code.

![Nosto debug category ](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-debug-toolbar-category.png)
