To support AJAX implementation of add to cart functionality inside product recommendations, the on-click handler of the add to cart button needs notify Nosto that a product was added to cart from a Nosto recommendation. The api call is only for informing Nosto about the attribution (that the product was added from the recommendation), normal cart tagging should be used to tell Nosto the user’s cart contents.


```js
nostojs(function(api) {
  api.recommendedProductAddedToCart("productId1", "nosto-categorypage-1");
});
```