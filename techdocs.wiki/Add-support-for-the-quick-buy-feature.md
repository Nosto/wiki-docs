If you would like your customers to be able to directly add recommended products to cart, you will need to implement an add-to-cart function.

Nosto already supports the add-to-cart feature out of the box on core platforms such as Magento 1 & 2, Shopify and Prestashop.

As a merchant looking to support the add-to-cart feature, you'll need to follow the next two parts:

## Exposing a JS function to add products

We recommend exposing a function named `Nosto.addProductToCart('<product-id')`. This function or (an equivalent one) must be responsible for making the POST (or an AJAX POST) request to add the specified product to the cart.

As a best practice, we advise that you add this function to your page and not into your recommendation template. Adding it to the page decouples it from the recommendation look and feel and allows the function to be accessible by all recommendations across your site.

## Amending the recommendation templates

The next step to amend the recommendation templates to contain a button that invokes the function.

**Note:** The default recommendation templates that ship with your account already has support for adding products to the cart via a function named `Nosto.addProductToCart`. If you have modified or deleted the template, please contact our support personnel to copy over a fresh template over to your account.