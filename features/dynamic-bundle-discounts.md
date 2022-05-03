# Dynamic Bundle Discounts

* [Dynamic Bundle discounts](dynamic-bundle-discounts.md#dynamic-bundle-discounts)
  * [Introduction](dynamic-bundle-discounts.md#introduction)
  * [Setup](dynamic-bundle-discounts.md#setup)
    * [Nosto Dynamic Bundle template](dynamic-bundle-discounts.md#nosto-dynamic-bundle-template)
    * [Shopify Line Item script](dynamic-bundle-discounts.md#shopify-line-item-script)
    * [Known Issues](dynamic-bundle-discounts.md#known-issues)

## Introduction

This documentation explains the process of setting up **Nosto - Dynamic Bundles** for automatic discount, when a bundle is added to the cart on a Shopify store.

## Setup

### Nosto Dynamic Bundle template

Nosto JavaScript API **addBundleToCartWithDiscount** can be used to add the selected bundle to the cart with the following parameters:

1. Products selected in the bundle while adding to cart (_items parameter_)
2. Discount (type & value) configuration (_discount parameter_)
3. All the product IDs from the bundle (both selected and unselected) (_bundleProducts parameter_)
4. A hash value that is provided with the template out of the box (_hash parameter_)

(a) An example where all the products from a bundle are added to cart.

```javascript
_targetWindow.Nosto.addBundleToCartWithDiscount({
    items: [{
        productId: '123456789',
        quantity: 1
    }, {
        productId: '987654321',
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

| Product ID | Price |
| :--------: | :---: |
|  123456789 |  126$ |
|  987654321 |  100$ |

**discount type** - percent
**discount value** - 10

_the final price customer pays: (126 \* 0.1) + (100 \* 0.1) => 113.4 + 90 => 203.4_

**Note**
Discount is applicable only when a bundle is added to the cart. Not applicable when products of a bundle are added individually.

(b) An example where NOT all the products from a bundle are added to cart.

```javascript
_targetWindow.Nosto.addBundleToCartWithDiscount({
    items: [{
        productId: '123456789',
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

_In the above example, no discount is offered and the customer pays the actual price of the selected product_

### Shopify Line Item script

Please follow the steps below for setting up the line item script for handling the bundle discount request. (_This has to be done manually for now, until next Nosto release_)

1. Please follow the instructions [here](https://help.shopify.com/en/manual/checkout-settings/script-editor/create) for installing and setting up Shopify Script Editor (_make sure to select blank template and clear any existing code in the template_)
2. Copy the code provided under attachments section and add it to the line item script that we created in step (1). This code authenticates bundle discount requests and applies the discount only for genuine requests.
3. The authentication logic has a GET\_FROM\_NOSTO variable. Value of this variable should be replaced with Nosto secret key. **To get your secret key, please contact Nosto support**
4. The code includes a commented testing section as shown below. This can be used to test the functionality of the bundle discount script. For testing, the script need to be unpublished in case if it's already published.
      - Uncomment from `Input.cart.line_times` till `end`
      - The hash value marked with (GET\_FROM\_DEV\_CONSOLE) can be retrieved from browser's network tab
      - Replace "PROD_1", "PROD_2" etc., with the actual product IDs.
      - `type` can be `percent` or `dollar`
      - **Make sure to comment the lines after testing and before publishing the script again**. This is an important step. Skipping this could cause issues with applying discounts in real-time.

\# ================================================================\
\# Testing block. Uncomment the following lines to test the script\
\#Input.cart.line_items.each do |line_item|\
\#  new_properties = { '_nosto_bundle' => { 'discount' => { 'type' => 'percent', 'value' => 10 }, 'bundle_products' => ["PROD_1", "PROD_2"], '_hash' => 'GET_FROM_DEV_CONSOLE' } }\
\#  line_item.change_properties(new_properties, { :message => "" })\
\#end\
\# ================================================================

### Known Issues

Nosto implementation uses a private property field for sharing discount information with Shopify. In newer theme versions, Shopify automatically hides these fields while displaying products in cart page. In older version of themes, these fields gets exposed as shown below.

**ADD IMAGE**

In such a case, make the following changes to the template liquid file that displays the cart information (usually cart.liquid or cart-template.liquid).

```html
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
```
