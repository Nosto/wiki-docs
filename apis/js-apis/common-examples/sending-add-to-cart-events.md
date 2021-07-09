# Including Add to Cart-buttons into Recommendations

Usually adding an Add to cart button directly into the recommendation which lists multiple products is fairly straightforward, the website just needs to have an appropriate javascript function to do the operation. Nosto provides ready-made functions for multiple platforms so check the Nosto documentation specific to your e-commerce platform or if needed look into your e-commerce platform's documentation and support on how to create such a function.

Once the Add to cart button is pressed it should call that function to add the products to the cart, but it should also additionally let Nosto know that the button was pressed so Nosto can track the sales from the recommendation. When using the JS API this attribution can be done with the `recommendedProductsAddedToCart`-function as follows:

```javascript
nostojs(function(api) {
  api.recommendedProductAddedToCart("productId1", "nosto-categorypage-1");
});
```

 ⚠️ The api call is only for informing Nosto about the attribution \(that the product was added from the recommendation\), normal cart tagging should be used to tell Nosto the user’s cart contents.

