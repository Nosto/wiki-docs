# Implementing Add To Cart

Nosto supports the add-to-cart feature out of the box on core platforms such as Magento 1 & 2, Shopify and Prestashop.

As a platform vendor an agency looking to support the add-to-cart feature, you'll need two parts:

1. A controller that accepts a product-id, a variation-id or a combination of both.
2. A javascript function that accepts the product-id, the variation-id and the reference to the clicked button

## Javascript Function

```text
if (typeof Nosto === "undefined") {
    var Nosto = {};
}
Nosto.addProductToCart = function(productId, element) {
    Nosto.trackAddToCartClick(productId, element);
    var fields = {
        "product": productId,
        "form_key": "<?php echo $formKey; ?>"
    };
    Nosto.postAddToCartForm(fields, "<?php echo $formAction; ?>");
};

// Product object must have fields productId and skuId {productId: 123, skuId: 321}
Nosto.addSkuToCart = function(product, element) {
    Nosto.trackAddToCartClick(product.productId, element);
    var fields = {
        "product": product.productId,
        "sku": product.skuId,
        "form_key": "<?php echo $formKey; ?>"
    };
    Nosto.postAddToCartForm(fields, "<?php echo $this->getAddSkuToCartUrl(); ?>");
};

Nosto.resolveContextSlotId = function(element) {
    var m = 20;
    var n = 0;
    var e = element;
    while (typeof e.parentElement !== "undefined" && e.parentElement) {
        ++n;
        e = e.parentElement;
        if (e.getAttribute('class') === 'nosto_element' && e.getAttribute('id')) {
            return e.getAttribute('id');
        }
        if (n >= m) {
            return false;
        }
    }
    return false;
};

Nosto.trackAddToCartClick = function(productId, element) {
    if (typeof nostojs !== 'undefined' && typeof element === 'object') {
        var slotId = Nosto.resolveContextSlotId(element);
        if (slotId) {
            nostojs(function(api) {
                api.recommendedProductAddedToCart(productId, slotId);
            });
        }
    }
};

Nosto.postAddToCartForm = function(data, url) {
    var form = document.createElement("form");
    form.setAttribute("method", "post");
    form.setAttribute("action", url);
    for (var key in data) {
        if (data.hasOwnProperty(key)) {
            var hiddenField = document.createElement("input");
            hiddenField.setAttribute("type", "hidden");
            hiddenField.setAttribute("name", key);
            hiddenField.setAttribute("value", data[key]);
            form.appendChild(hiddenField);
        }
    }
    document.body.appendChild(form);
    form.submit();
};
```

The Javascript function must be exposed on the parent window so it that it can be directly invoked using `Nosto.addProductToCart` and `Nosto.addSkuToCart`.

## Controller Endpoint

Most platforms ship with an add-to-cart controller that can be reused. \(It is irrelevant whether the controller is an Ajax endpoint or not. How you update your mini-cart is highly platform specific.\)

The Javascript function posts the product-id to the controller to add the product to the cart. Here are some examples of the built-in add to cart controller in some platforms

Here's an example of the built-in Magento add-to-cart controller that requires not only identifier but also the attributes of the product being added. The URL has an embedded token and the form post parameters have a CSRF token. This makes the controller hard to reuse.

![screen shot 2017-10-19 at 15 22 41](https://user-images.githubusercontent.com/327432/31770602-674495f8-b4e1-11e7-8372-392be2cccec0.png)

Here's an example of the built-in JTL add-to-cart controller that requires not only identifier but also the attributes of the product being added. The URL is the URL of the product being added and the form post parameters have a CSRF token. This makes the controller hard to reuse.

![screen shot 2017-10-19 at 15 20 55](https://user-images.githubusercontent.com/327432/31770562-2ca536dc-b4e1-11e7-9a9a-a3b2a9386ccd.png)

### Custom Controllers

If your controller requires additional parameters such the attribute \(size and color\) of the product being added, you may need to build a custom controller as these attributes aren't easily accessible in the recommendation slot. The examples that shown above highlight some of the complexities involved in reusing the built-in add to cart controller.

If you do create a custom controller, here are some proven guidelines. Your custom controller should require only the following fields:

* Product Id. / Variation Id. \(or both\)
* The CSRF \(security\) token
* The quantity

Looking at custom add-to-cart controller for [Magento](https://github.com/Nosto/nosto-magento/blob/develop/app/code/community/Nosto/Tagging/controllers/AddToCartController.php) should be a good reference point.

### CSRF Protection

If you are reusing an existing controller, the endpoint may have some sort of CSRF protection. The CSRF token generally exists on the page and you should be able to extract it via Javascript before doing the POST. Alternatively, you could render the Javascript snippet onto the page with the CSRF token rendered into the script.

