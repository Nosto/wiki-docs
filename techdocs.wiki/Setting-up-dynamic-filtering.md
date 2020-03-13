In this article, you will learn to use certain tags and fields to dynamically filter and facet your recommendation results.

You can use any combination of the different filtering mechanisms outlined below. For example, you can use category and the tag filtering to constrain the recommendation results.

**Note:** In order to leverage dynamic filtering please read our guide on configuring the filtering behaviour.

All category pages already leverage dynamic filtering. As documented in our guide to tagging categories, the `nosto_category` tagging constraints Nosto to show recommendations from only the current category.

Tagging the current category is often for most retailers to add personalisation to the category pages. If your store uses faceting and you would like Nosto to respect the faceting constraints, you may need to use either the attribute or tag filtering mechanisms to achieve the desired result.



#### **Filtering by categories**

You can filter by categories to narrow down the recommendation results to only show products from the specified category or categories. If multiple categories are specified, the products must be in each of those categories.

```html
<div class="nosto_category" style="display:none">/Men's/Shirts</div>
```

You can even use multiple

```html
<div class="nosto_category" style="display:none">/Men's/Shirts</div>
<div class="nosto_category" style="display:none">/Men's/Sale</div>
```

**Note:** Remember to tag the categories exactly as they are tagged in your product pages. If you've omitted the leading `/Home` from your category tagging on the product pages, you'll need to tag them in a similar format here.

#### Filtering by tags

You can filter by tags to narrow down the recommendation results to only show products containing the specified tag or tags. If multiple tags are specified, the products must contain all the specified tags.

```html
<div class="nosto_tag" style="display:none">colourful</div>
```

You can even use multiple

```html
<div class="nosto_tag" style="display:none">colourful</div>
<div class="nosto_tag" style="display:none">shiny</div>
```

#### Filtering by attributes

You can filter by attributes to narrow down the recommendation results to only show products containing the specified attributes. If multiple attributes are specified, the products must contain all the attributes.

```html
<div class="nosto_custom_field" style="display:none">gender:male</div>
```

You can even use multiple

```html
<div class="nosto_custom_field" style="display:none">gender:male</div>
<div class="nosto_custom_field" style="display:none">material:cotton</div>
```

## Dynamically reloading

If you want to refresh the recommendations with new facet constraints, the simplest way would be to update the value of the filters in the page and then reload the recommendations.

The following example illustrates a simple way of modifying the current category tagging and then using the JS API to reload the recommendations.

```js
document.querySelector('.nosto_category').innerText = '/Shoes'

nostojs(function(api) {
  api.loadRecommendations();
});
```