# Dynamic Bundle discounts

- [Dynamic Bundle discounts](#dynamic-bundle-discounts)
  - [Introduction](#introduction)
  - [Setup](#setup)
    - [Nosto Dynamic Bundle template](#nosto-dynamic-bundle-template)
    - [Shopify Line Item script](#shopify-line-item-script)
    - [Known Issues](#known-issues)

## Introduction
This documentation explains the process of setting up **Nosto - Dynamic Bundles** for automatic discount, when a bundle is added to the cart on a Shopify store. 

## Setup

### Nosto Dynamic Bundle template
A new Nosto JavaScript API **addBundleToCartWithDiscount** has been introduced. 

In order to use this new API, invoke the API from the dynamic bundle template with the following parameters,
1. Products selected in the bundle while adding to cart (_items parameter_)
2. Discount (type & value) configuration (_discount parameter_)
3. All the product IDs from the bundle (both selected and unselected) (_bundleProducts parameter_)

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
    bundleProducts: [ '123456789', '987654321' ]
}, this)
```
in the above example, the configured discount value applies to all the products in the bundle. <br>

Assume for example, 

| Product ID | Price |
| :-----------: | :-----: |
| 123456789 | 126$ |
| 987654321 | 100$ |

**discount type** - percent <br>
**discount value** - 10

_the final price the customer pays is, (126 * 0.1) + (100 * 0.1) => 113.4 + 90 => 203.4_

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
    bundleProducts: [ '123456789', '987654321' ]
}, this)
```

_In the above example, no discount is offered and the customer pays the actual price of the selected product_

### Shopify Line Item script
Please follow the steps below for setting up the line item script for handling the bundle discount request. (_This has to be done manually for now, until next Nosto release_)

1. Please follow the instructions [here](https://help.shopify.com/en/manual/checkout-settings/script-editor/create) to install and setup Shopify Script Editor (_make sure to select blank template and clear any existing code in the template_)
2. Copy the authentication logic (entire code) from [here](https://github.com/ripenecommerce/shopify-ruby-sha256/blob/main/sha265.rb) and add it to the line item script that we created in step (1). This code authenticates bundle discount requests and applies the discount only for genuine requests.
3. Copy the shopify bundle discount line item script from [here](https://help.shopify.com/en/manual/checkout-settings/script-editor/examples/line-item-scripts#bundle-discount) and add it below the authentication logic.
4. The authentication logic has a SECRET_KEY variable. Value of this variable should be replaced with Nosto secret key. **To get your secret key, please contact Nosto support**
5. Finally copy the code below to the line item script in order to complete the setup. 
> Note: The code presents a testing block. This can been uncommented for testing the script in script editor
   
```ruby
BUNDLE_ITEMS = []

discount_type = ""
discount_amount = 0.0

# ================================================================
# Testing block. Uncomment the following lines to test the script
#
#Input.cart.line_items.each do |line_item|
#  new_properties = { '_nosto_bundle' => { 'discount' => { 'type' => 'percent', 'value' => 10 }, 'bundle_products' => ["7513894387937", "7513894191329"] } }
#  line_item.change_properties(new_properties, { :message => "" })
#end
# ================================================================

class NostoBundleHandler
  def initialize(line_items)
    @nosto_bundle_products = []
    if line_items.length() > 0
      @nostoBundle = line_items.first.properties["_nosto_bundle"]
      if @nostoBundle.nil? == false
        @nosto_bundle_products = @nostoBundle["bundle_products"]
        cleanup()
      end
    end
  end
  
  def cleanup()
    @nosto_bundle_products = @nosto_bundle_products.map do |item| 
      if item.class == Integer
        item
      else
        item.to_i
      end
    end
  end
  
  def isBundleActive()
    cart_product_ids = Input.cart.line_items.map {
      |item|item.variant.product.id 
    }
    (@nosto_bundle_products.to_a - cart_product_ids).empty?
  end
  
  def getDiscount()
    if @nostoBundle.nil? == false
      @nostoBundle['discount']
    else
      nil
    end
  end
end

nostoBundleHandler = NostoBundleHandler.new(Input.cart.line_items)
isBundleActive = nostoBundleHandler.isBundleActive()

Input.cart.line_items.each do |line_item|
  bundle_item = {}
    
  if isBundleActive
    bundle_item[:product_id] = line_item.variant.product.id
    bundle_item[:quantity_needed] = line_item.quantity
    
    discountConfig = nostoBundleHandler.getDiscount()
    
    if discount_type.empty?  
      unless discountConfig.nil?
        discountType = discountConfig['type']
        discount_type = discountType == 'percent' ? :percent : :dollar
        discount_amount = discountConfig['value']
      end
    end
    BUNDLE_ITEMS << bundle_item
  end
end

@BUNDLE_DISCOUNTS = [{
  :bundle_items => BUNDLE_ITEMS,
  :discount_type => discount_type,
  :discount_amount => discount_amount,
  :discount_message => "#{discount_amount} #{discount_type} bundle discount!"
}]


if BUNDLE_ITEMS.empty? == false and BUNDLE_ITEMS.nil? == false
  CAMPAIGNS = [
    BundleDiscountCampaign.new(@BUNDLE_DISCOUNTS)
  ]
  
  CAMPAIGNS.each do |campaign|
    campaign.run(Input.cart)
  end
end

Output.cart = Input.cart
```

### Known Issues
Nosto implementation uses a private property field for sharing discount information with Shopify. In newer theme versions, Shopify automatically hides these fields while displaying products in cart page. In older version of themes, these fields gets exposed as shown below.

__ADD IMAGE__

In such a case, we can add the following line to the template liquid file that displays the cart information (usually cart-template.liquid). 

```html
{%- assign property_size = item.properties | size -%}
{% if property_size > 0 %}
    <div class="cart__meta-text">
    {% for p in item.properties %}
        {%- assign property_first_char = p.first | slice: 0 -%} <== new line
        {%- if p.last != blank and property_first_char != '_' -%} <== new line
            {{ p.first }}:

            {% comment %}
                Check if there was an uploaded file associated
            {% endcomment %}
            {% if p.last contains '/uploads/' %}
                <a href="{{ p.last }}">{{ p.last | split: '/' | last }}</a>
            {% else %}
                {{ p.last }}
            {% endif %}
        {% endif %} <== new line
    {% endfor %}
    </div>
{% endif %}
```