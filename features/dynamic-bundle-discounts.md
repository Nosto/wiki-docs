# Dynamic Bundle Discounts

* [Dynamic Bundle discounts](dynamic-bundle-discounts.md#dynamic-bundle-discounts)
  * [Introduction](dynamic-bundle-discounts.md#introduction)
  * [Setup](dynamic-bundle-discounts.md#setup)
    * [Nosto Dynamic Bundle template](dynamic-bundle-discounts.md#nosto-dynamic-bundle-template)
    * [Shopify Line Item script](dynamic-bundle-discounts.md#shopify-line-item-script)
  * [Important Note](dynamic-bundle-discounts.md#important-note)
  * [Known Issues](dynamic-bundle-discounts.md#known-issues)

## Introduction

This documentation explains the process of setting up **Nosto - Dynamic Bundles** for automatic discount, when a bundle is added to the cart on a Shopify store.

## Setup

### Nosto Dynamic Bundle template

Nosto bundle templates needs the following mandatory variables (name intact) to be configured for proper functioning:
1. discount type - Defines the type of discount offered (percent/dollar)
2. discount value - Defines the amount/percent of discount offered on products
3. secured - Enable (true)/Disable (false) `hash` based authentication for bundle discounts. 
4. discountCurrentProduct - Defines whether the currently displayed product in PDP to be included & discounted when an associated bundle is added to cart. This flag is useful when bundles are displayed along with product details as depicted in image below:

![PDP with Dynamic Bundle](https://user-images.githubusercontent.com/82023195/173544975-2b80f502-fe2c-41d8-b388-11ce8f951db2.png)

In additional to these mandatory variables, any number of additional custom variables can be created and configured. A sample bundle template variable configuration is shown below

![Dynamic Bundle template variables](https://user-images.githubusercontent.com/82023195/173543873-dcbc9224-3410-4e52-8d72-4f60067fbcfa.png)

In the above sample configuration, all the mandatory fields are highlighed and the "ATC button text" and "subtitle" are custom variables

Nosto JavaScript API **addBundleToCartWithDiscount** can be used to add the selected bundle to the cart with the following parameters:

1. Products selected in the bundle while adding to cart (_items parameter_)
2. Discount (type & value) configuration (_discount parameter_)
3. All the product IDs from the bundle (both selected and unselected) (_bundleProducts parameter_)
4. A hash value that is provided with the template out of the box (_hash parameter_)

Expanding _items parameter_, it should contain the following fields:

1. `productId` - Product ID of the variant selected in the bundle
2. `variantId` - ID of the selected variant in the bundle
3. `quantity` - An optional field indicating product quantity offered in bundle. Defaults to 1. For bundles with only one quantity, this field can be omitted

(a) An example where all the products from a bundle are added to cart.

```javascript
_targetWindow.Nosto.addBundleToCartWithDiscount({
    items: [{
        productId: '123456789',
        variantId: '4123456098',
        quantity: 3
    }, {
        productId: '987654321',
        variantId: '4897122347',
        quantity: 1
    }],
    discount: {
        type: 'percent',
        value: 10
    },
    bundleProducts: [ '123456789', '987654321' ],
    hash: "$hash"
}, this)
```

in the above example, the configured discount value applies to all the products in the bundle.

Assume for example,

| Product ID | Variant ID | Price |
| :--------: | :--------: | :---: |
|  123456789 | 4123456098 |  126$ |
|  987654321 | 4897122347 |  100$ |

**discount type** - percent **discount value** - 10

_the final price customer pays: (126 \* 3 \* 0.1) + (100 \* 1 \* 0.1) => 340.2 + 90 => 430.2_

**Note:**\
Discount is applicable only when a bundle is added to the cart. Not applicable when products of a bundle are added individually.

(b) An example where NOT all the products from a bundle are added to cart.

```javascript
_targetWindow.Nosto.addBundleToCartWithDiscount({
    items: [{
        productId: '123456789',
        variantId: '4123456098'
    }],
    discount: {
        type: 'percent',
        value: 10
    },
    bundleProducts: [ '123456789', '987654321' ],
    hash: "$hash"
}, this)
```

_In the above example, quantity defaults to 1 and no discount is offered and the customer pays the actual price of the selected product_

### Shopify Line Item script

Please follow the steps below for setting up the line item script for handling the bundle discount request. (_This has to be done manually for now, until next Nosto release_)

1. Please follow the instructions [here](https://help.shopify.com/en/manual/checkout-settings/script-editor/create) for installing and setting up Shopify Script Editor (_make sure to select blank template and clear any existing code in the template_)
2. Copy the code from [here](https://github.com/Nosto/wiki-docs/files/8706014/line\_item\_script.txt) and add it to the line item script that we created in step (1). This code authenticates bundle discount requests and applies the discount only for genuine requests.
3. The authentication logic has a GET\_FROM\_NOSTO variable. Value of this variable should be replaced with Nosto secret key. **To get your secret key, please contact Nosto support**
4. The code includes a commented testing section as shown below. This can be used to test the functionality of the bundle discount script. For testing, the script need to be unpublished in case if it's already published.
   * Uncomment from `Input.cart.line_times` till `end`
   * The hash value marked with (GET\_FROM\_DEV\_CONSOLE) can be retrieved from browser's network tab
   * Replace "PROD\_1", "PROD\_2" etc., with the actual product IDs.
   * `type` can be `percent` or `dollar`
   * **Make sure to comment the lines after testing and before publishing the script again**. This is an important step. Skipping this could cause issues with applying discounts in real-time.

\# ================================================================\
\# Testing block. Uncomment the following lines to test the script\
\#Input.cart.line\_items.each do |line\_item|\
\# new\_properties = { '\_nosto\_bundle' => { 'discount' => { 'type' => 'percent', 'value' => 10 }, 'bundle\_products' => \["PROD\_1", "PROD\_2"], '\_hash' => 'GET\_FROM\_DEV\_CONSOLE' } }\
\# line\_item.change\_properties(new\_properties, { :message => "" })\
\#end\
\# ================================================================

## Important Note
Nosto Dynamic Bundle configuration involves two important keys, hash and secret key. Hash key is used within the bundle template and can be accessed using predefined variation `$hash`. Please avoid hard-coding this value anywhere inside the template. As dynamic bundle configurations are subjected to change, the hash key will also change accordingly. Hard-coding this key may break the functionality.

Seret key, on the other hand, can be retrieved from "Dynamic Bundle Key" field in Nosto Admin Settings > Platform page. This value replaces GET\_FROM\_NOSTO placeholder in the Shopify line item script.

## Known Issues

Nosto implementation uses a private property field for sharing discount information with Shopify. In newer theme versions, Shopify automatically hides these fields while displaying products in cart page. In older version of themes, these fields gets exposed as shown below.

![Properties Exposed](https://user-images.githubusercontent.com/82023195/168611801-666c24bc-c7f9-4edd-bdf5-2be32798391c.png)

In such a case, make the following changes to the template liquid file that displays the cart information (usually cart.liquid or cart-template.liquid).

```html
{% raw %}
{% for p in item.properties %}
  {% assign first_character_in_key = p.first | truncate: 1, '' %} <== new line
  {% unless p.last == blank or first_character_in_key == '_' %} <== new line
  <small>
    {{ p.first }}:

    {% comment %}
      Check if there was an uploaded file associated
    {% endcomment %}
    {% if p.last contains '/uploads/' %}
      <a href="{{ p.last }}">{{ p.last | split: '/' | last }}</a>
    {% else %}
      {{ p.last }}
    {% endif %}
  </small><br />
  {% endunless %} <== new line
{% endfor %}
{% endraw %}
```
