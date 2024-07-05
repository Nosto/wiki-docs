# Dynamic Bundle Discounts

* [Dynamic Bundle Discounts](dynamic-bundle-discounts.md#dynamic-bundle-discounts)
  * [Introduction](dynamic-bundle-discounts.md#introduction)
  * [Setup](dynamic-bundle-discounts.md#setup)
    * [Nosto Dynamic Bundle template](dynamic-bundle-discounts.md#nosto-dynamic-bundle-template)
    * [Shopify Line Item script](dynamic-bundle-discounts.md#shopify-line-item-script)
    * [Debugging Line Item Script](dynamic-bundle-discounts.md#debugging-line-item-script)
  * [Important Note](dynamic-bundle-discounts.md#important-note)
  * [Nosto Bundle Script](dynamic-bundle-discounts.md#nosto-bundle-script)
  * [Troubleshooting](dynamic-bundle-discounts.md#troubleshooting)
    * [Issue #1: Additional content displayed on cart page](dynamic-bundle-discounts.md#issue-1-additional-content-displayed-on-cart-page)
    * [Cause](dynamic-bundle-discounts.md#cause)
    * [Solution](dynamic-bundle-discounts.md#solution)
    * [Issue #2: Discount is not applied](dynamic-bundle-discounts.md#issue-2-discount-is-not-applied)
    * [Cause](dynamic-bundle-discounts.md#cause-1)
    * [Solution](dynamic-bundle-discounts.md#solution-1)

## Introduction

This documentation explains the process of setting up **Nosto - Dynamic Bundles** for automatic discount, when a bundle is added to the cart on a Shopify store.

{% hint style="warning" %}
This functionality can only be used by Shopify Plus merchants.&#x20;
{% endhint %}

## Setup

### Nosto Dynamic Bundle template

Nosto bundle templates needs the following mandatory variables (name intact) to be configured for proper functioning:

1. discount type - defines the type of discount offered (percent/dollar)
2. discount value - defines the amount/percent of discount offered on products
3. secured - Enable (true)/Disable (false) `hash` based authentication for bundle discounts.
4. discount current product - Defines whether the currently displayed product in PDP to be included & discounted when an associated bundle is added to cart. This flag is useful when bundles are displayed along with product details as depicted in image below:

![PDP with Dynamic Bundle](https://user-images.githubusercontent.com/82023195/173544975-2b80f502-fe2c-41d8-b388-11ce8f951db2.png)

A sample bundle template variable configuration is shown below

![Dynamic Bundle template variables](https://user-images.githubusercontent.com/82023195/173543873-dcbc9224-3410-4e52-8d72-4f60067fbcfa.png)

In the above sample configuration, all the mandatory fields are highlighted and the "ATC button text" and "subtitle" are custom variables

Variables can be added to the template using the following approach. Please note, this section only outlines the steps for adding variables and not creating the template end-to-end.

1. Navigate to "Recommendations" > "Templates" and click "Create template" button.
2. Select the "Variables" tab.
3. For adding "discount type" variable, populate the name and type field as shown below and click "Add"

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>"discount type" field</p></figcaption></figure>

4. Click the <img src="../.gitbook/assets/image (8).png" alt="" data-size="line">icon next to the "discount type" field and make sure to add the details as given in the screenshot below

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

4. For adding "discount value" variable, populate the name and type field as shown below and click "Add"

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>"discount value" field</p></figcaption></figure>

6. Click the <img src="../.gitbook/assets/image (8).png" alt="" data-size="line">icon next to "discount value" field and make sure to add the details as given in the screenshot below

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Adding "discount value" variable</p></figcaption></figure>

7. For adding "secured" variable, populate the name and type field as shown below and click "Add"

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>"secured" field</p></figcaption></figure>

8. Click the <img src="../.gitbook/assets/image (8).png" alt="" data-size="line">icon next to "secured" field and make sure to add the details as given in the screenshot below

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>Adding "secured" field</p></figcaption></figure>

9. For adding "discount current product" variable, populate the name and type field as shown below and click "Add"

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>"discount current product" field</p></figcaption></figure>

10. Click the <img src="../.gitbook/assets/image (8).png" alt="" data-size="line">icon next to "discount current product" field and make sure to add the details as given in the screenshot below

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p>Adding "discount current product" field</p></figcaption></figure>

Nosto JavaScript API **addBundleToCartWithDiscount** can be used to add the selected bundle to the cart with the following parameters:

1. Products selected in the bundle while adding to cart (_items parameter_)
2. Discount (type & value) configuration (_discount parameter_)
3. All the product IDs from the bundle (both selected and not selected) (_bundleProducts parameter_)
4. A hash value that is provided with the template out of the box (_hash parameter_). This hash value will later be used in conjunction with the secret key obtained from Nosto in order to authenticate the request for bundle discount.

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

Please follow the steps below for setting up the line item script for handling the bundle discount request.

1. Please follow the instructions [here](https://help.shopify.com/en/manual/checkout-settings/script-editor/create) for installing and setting up Shopify Script Editor (_make sure to select blank template and clear any existing code in the template_)
2. Copy the code from [here](dynamic-bundle-discounts.md#authentication-script) and add it to the line item script that we created in step (1). This code authenticates bundle discount requests and applies the discount only for genuine requests.
3. Copy the Nosto bundle discount script from [here](dynamic-bundle-discounts.md#nosto-bundle-script) and add it to the line item script, below the authentication script (added from previous step)
4. The authentication logic has a GET\_FROM\_NOSTO variable. Value of this variable should be replaced with Nosto secret key. **To get your secret key, please contact Nosto support**
5. Click"Save and Publish" to publish the script

### Debugging Line Item Script

[Nosto Bundle Script](dynamic-bundle-discounts.md#nosto-bundle-script) includes a commented testing section as shown below. This can be used to test the functionality of the bundle discount script. For testing, the script need to be unpublished in case if it's already published.

* Uncomment from `Input.cart.line_times` till `end`
* The hash value marked with (GET\_FROM\_DEV\_CONSOLE) can be retrieved from browser's network tab after adding a bundle to cart
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

Nosto Dynamic Bundle configuration involves two important keys, hash and secret key. Hash key is used within the bundle template and can be accessed using predefined variable `$hash`. Please avoid hard-coding this value anywhere inside the template. As dynamic bundle configurations are subjected to change, the hash key will also change accordingly. Hard-coding this key may break the functionality.

Secret key, on the other hand, can be retrieved from "Dynamic Bundle Key" field in Nosto Admin Settings > Platform page. This value replaces GET\_FROM\_NOSTO placeholder in the Shopify line item script.

````
## [Authentication Script](#authentication-script)

```ruby
# Convert integer to binary string (32 bits) - modified from original
def bits(x, n = 32)
  if x >= 0
    return x.to_s(2).rjust(n, '0') # "%0#{n}b" % x does not work in Shopify
  else
    # Note: Ruby NOT function returns a negative number, and .to_s(2) displays this mathematical representation in base 2.
    # Note: So to get the expected unsigned notation you need to get the individual bits instead.
    # Note: When doing so, ignore the first bit because that's the sign bit.
    # https://www.calleerlandsson.com/rubys-bitwise-operators/
    return (n - 1).downto(0).map { |i| x[i] }.join
  end
end

# Convert integer to hexadecimal string (32 bits)
def hex(i)
  return i.to_s(16).rjust(8, "0")
end

# Convert string to binary string
def bitstring(string)
  bytes = string.bytes                  # convert ascii characters to bytes (integers)
  binary = bytes.map { |x| bits(x, 8) } # convert bytes to binary strings (8 bits in a byte)
  return binary.join
end

# Convert input (hex, ascii) to array of bytes
def bytes(input, type)
  case type
    # Removed unused "binary" type from original
    when "hex"
      hex = input[2..-1] # trim 0x prefix
      bytes = [hex].pack("H*").unpack("C*") # convert hex string to bytes
    else
      bytes = input.bytes # convert ASCII string to bytes
  end

  return bytes
end

# ----------
# Operations
# ----------
# Addition modulo 2**32
def add(*x)
  total = x.inject(:+)
  return total % 2 ** 32 # limits result of addition to 32 bits
end

# Rotate right (circular right shift)
def rotr(n, x)
  right = (x >> n)              # right shift
  left = (x << 32 - n)          # left shift
  result = right | left         # combine to create rotation effect
  return result & (2 ** 32 - 1) # use mask to truncate result to 32 bits
end

# Shift right
def shr(n, x)
  result = x >> n
  return result
end

# ---------
# Functions - Combined rotations and shifts using operations above
# ---------
def sigma0(x)
  return rotr(7, x) ^ rotr(18, x) ^ shr(3, x)
end
def sigma1(x)
  return rotr(17, x) ^ rotr(19, x) ^ shr(10, x)
end
def usigma0(x)
  return rotr(2, x) ^ rotr(13, x) ^ rotr(22, x)
end
def usigma1(x)
  return rotr(6, x) ^ rotr(11, x) ^ rotr(25, x)
end

# Choice - Use first bit to choose the (1)second or (0)third bit
def ch(x, y, z)
  return (x & y) ^ (~x & z)
end

# Majority - Result is the majority of the three bits
def maj(x, y, z)
  return (x & y) ^ (x & z) ^ (y & z)
end


# -------------
# Preprocessing
# -------------
# Pad binary string message to multiple of 512 bits
def padding(message)
  l = message.size  # size of message (in bits)
  k = (448 - l - 1) % 512 # pad with zeros up to 448 bits (64 bits short of 512 bits)
  l64 = bits(l, 64) # binary representation of message size (64 bits in length)
  return message + "1" + ("0" * k) + l64 # don't forget "1" bit between message and padding
end

# Cut padded message in to 512-bit message blocks - modified
def split(message, size = 512)
  return (0..(message.length-1)/size).map{|i|message[i*size,size]} # message.scan(/.{#{size}}/)
end

# ----------------
# Message Schedule
# ----------------
# Calculate the 64 words for the message schedule from the message block
def calculate_schedule(block)
  # The message block provides the first 16 words for the message schedule (512 bits / 32 bits = 16 words)
  schedule = split(block,32).map { |w| w.to_i(2) } # convert from binary string to integer for calculations

  # Calculate remaining 48 words
  16.upto(63) do |i|
    schedule << add(sigma1(schedule[i - 2]), schedule[i - 7], sigma0(schedule[i - 15]), schedule[i - 16])
  end

  return schedule
end


# ---------
# Constants
# ---------
# Constants = Cube roots of the first 64 prime numbers (first 32 bits of the fractional part)
K = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311].map { |prime| prime ** (1 / 3.0) }.map { |i| (i - i.floor) }.map { |i| (i * 2 ** 32).floor }

# -----------
# Compression - Run compression function on the message schedule and constants
# -----------
# Initial Hash Values = Square roots of the first 8 prime numbers (first 32 bits of the fractional part)
IV = [2, 3, 5, 7, 11, 13, 17, 19].map { |prime| prime ** (1 / 2.0) }.map { |i| (i - i.floor) }.map { |i| (i * 2 ** 32).floor }
def compression(initial, schedule, constants)
  # state register - set initial values ready for the compression function
  h = initial[7]
  g = initial[6]
  f = initial[5]
  e = initial[4]
  d = initial[3]
  c = initial[2]
  b = initial[1]
  a = initial[0]

  # compression function - update state for every word in the message schedule
  64.times do |i|
    # calculate temporary words
    t1 = add(schedule[i], constants[i], usigma1(e), ch(e, f, g), h)
    t2 = add(usigma0(a), maj(a, b, c))

    # rotate state registers one position and add in temporary words
    h = g
    g = f
    f = e
    e = add(d, t1)
    d = c
    c = b
    b = a
    a = add(t1, t2)
  end

  # Final hash values are previous intermediate hash values added to output of compression function
  hash = []
  hash[7] = add(initial[7], h)
  hash[6] = add(initial[6], g)
  hash[5] = add(initial[5], f)
  hash[4] = add(initial[4], e)
  hash[3] = add(initial[3], d)
  hash[2] = add(initial[2], c)
  hash[1] = add(initial[1], b)
  hash[0] = add(initial[0], a)

  # return final state
  return hash
end

# -------
# SHA-256 - Complete SHA-256 function
# -------
def sha256(string)
  # 0. Convert String to Binary
  # ---------------------------
  message = bitstring(string)

  # 1. Preprocessing
  # ----------------
  # Pad message
  padded = padding(message)

  # Split up in to 512 bit message blocks
  blocks = split(padded, 512)

  # 2. Hash Computation
  # -------------------
  # Set initial hash state using initial hash values
  hash = IV

  # For each message block
  blocks.each do |block|
    # Prepare 64 word message schedule
    schedule = calculate_schedule(block)

    # Remember starting hash values
    initial = hash.clone

    # Apply compression function to update hash values
    hash = compression(initial, schedule, constants = K)
  end

  # 3. Result
  # ---------
  # Convert hash values to hexadecimal and concatenate
  return hash.map { |w| w.to_s(16).rjust(8, '0') }.join
end

# Secret Key can be anything but must be consistent between this script and the frontend
SECRET_KEY="GET_FROM_NOSTO"
````

## [Nosto Bundle Script](dynamic-bundle-discounts.md#nosto-bundle-script)

{% hint style="danger" %}
We offer support only when the script, given below, is used as-is. We don't offer support when the below script is modified, in any way.
{% endhint %}

```ruby
# ================================ Script Code (do not edit) ================================
# ================================================================
# BundleSelector
#
# Finds any items that are part of the entered bundle and saves
# them.
# ================================================================
class BundleSelector
  def initialize(bundle_items)
    @bundle_items = bundle_items.reduce({}) do |acc, bundle_item|
      acc[bundle_item[:product_id]] = {
        cart_items: [],
        quantity_needed: bundle_item[:quantity_needed],
        total_quantity: 0,
      }

      acc
    end
  end

  def build(cart)
    cart.line_items.each do |line_item|
            next if line_item.line_price_changed?
      next unless @bundle_items[line_item.variant.product.id]

      @bundle_items[line_item.variant.product.id][:cart_items].push(line_item)
      @bundle_items[line_item.variant.product.id][:total_quantity] += line_item.quantity
    end

    @bundle_items
  end
end

# ================================================================
# DiscountApplicator
#
# Applies the entered discount to the supplied line item.
# ================================================================
class DiscountApplicator
  def initialize(discount_type, discount_amount, discount_message)
    @discount_type = discount_type
    @discount_message = discount_message

    @discount_amount = if discount_type == :percent
      1 - (discount_amount * 0.01)
    else
      Money.new(cents: 100) * discount_amount
    end
  end

  def apply(line_item)
    new_line_price = if @discount_type == :percent
      line_item.line_price * @discount_amount
    else
      [line_item.line_price - (@discount_amount * line_item.quantity), Money.zero].max
    end

    line_item.change_line_price(new_line_price, message: @discount_message)
  end
end

# ================================================================
# DiscountLoop
#
# Loops through the supplied line items and discounts the supplied
# number of items by the supplied discount.
# ================================================================
class DiscountLoop
  def initialize(discount_applicator)
    @discount_applicator = discount_applicator
  end

  def loop_items(cart, line_items, num_to_discount)
    line_items.each_with_index do |line_item|
      break if num_to_discount <= 0

      if line_item.quantity > num_to_discount
        split_line_item = line_item.split(take: num_to_discount)
        @discount_applicator.apply(split_line_item)
        position = cart.line_items.find_index(line_item)
        cart.line_items.insert(position + 1, split_line_item)
        break
      else
        @discount_applicator.apply(line_item)
        num_to_discount -= line_item.quantity
      end
    end
  end
end

# ================================================================
# BundleDiscountCampaign
#
# If the entered bundle is present, the entered discount is
# applied to each item in the bundle.
# ================================================================
class BundleDiscountCampaign
  def initialize(campaigns)
    @campaigns = campaigns
  end

  def run(cart)
    @campaigns.each do |campaign|
      bundle_selector = BundleSelector.new(campaign[:bundle_items])
      bundle_items = bundle_selector.build(cart)

      next if bundle_items.any? do |product_id, product_info|
        product_info[:total_quantity] < product_info[:quantity_needed]
      end

      num_bundles = bundle_items.map do |product_id, product_info|
        (product_info[:total_quantity] / product_info[:quantity_needed])
      end

      num_bundles = num_bundles.min.floor

      discount_applicator = DiscountApplicator.new(
        campaign[:discount_type],
        campaign[:discount_amount],
        campaign[:discount_message]
      )

      discount_loop = DiscountLoop.new(discount_applicator)

      bundle_items.each do |product_id, product_info|
        discount_loop.loop_items(
          cart,
          product_info[:cart_items],
          (product_info[:quantity_needed] * num_bundles),
        )
      end
    end
  end
end


BUNDLE_ITEMS = []

discount_type = ""
discount_amount = 0.0

# ================================================================
# Testing block. Uncomment the following lines to test the script
#

#Input.cart.line_items.each do |line_item|
#  new_properties = { '_nosto_bundle' => { 'discount' => { 'type' => 'dollar', 'value' => 10 }, 'bundle_products' => ["9172269958", "9172630790", "4719799304243"], '_hash' => '239a2b5bc60229ca8a6d9dd0f390a5bdc7098996506415e8637d3381e1935269' } }
#  new_properties = { '_nosto_bundle' => { 'discount' => { 'type' => 'percent', 'value' => 5 }, 'bundle_products' => ["9172269958", "9172630790", "4719799304243"], '_hash' => '239a2b5bc60229ca8a6d9dd0f390a5bdc7098996506415e8637d3381e1935269' } }
#  line_item.change_properties(new_properties, { :message => "discount test" })
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
  
  def isValid()
    if @nostoBundle.to_a.empty? or @nostoBundle['_hash'].to_s.nil?
      return false
    end
    data = @nosto_bundle_products.to_a.join(":")
    if data.empty?
      return false
    end
    discount = getDiscount()
    if discount.nil?
      return false
    end
    if discount['type'].nil? or discount['type'].to_s.strip.empty?
      return false
    end
    if discount['value'].nil? or discount['value'].to_s.strip.empty?
      return false
    end
    hashData = [data, discount['type'], discount['value'], SECRET_KEY].join(":")
    SHA_HASH = sha256(hashData)
    return SHA_HASH == @nostoBundle['_hash']
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
  
  def isBundleItem(productId)
    @nosto_bundle_products.to_a.include?(productId)
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

if nostoBundleHandler.isValid()
  
  isBundleActive = nostoBundleHandler.isBundleActive()
  
  if isBundleActive
  
    Input.cart.line_items.each do |line_item|
      
      product_id = line_item.variant.product.id
      
      if nostoBundleHandler.isBundleItem(product_id)
      
        bundle_item = {}
        
        bundle_item[:product_id] = product_id
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
end

Output.cart = Input.cart
```

## Nosto recommendation template sample

Given below is an example of a Nosto recommendation template for Dynamic Bundle discount. It's only for reference purposes and can't be used in production as it is.&#x20;

### Points to consider

1. **$Hash** value for bundle discount is generated following a particular order of values. The order goes like this
   1. All the Product Ids returned from the Recommendation
   2. Product ID of the currently viewed product (only if **discount current product** is **true**)
   3. Value of **discount type** variable (percent or dollar)
   4. Value of **discount value** variable
2. The ordering above should be maintained while triggering the _**addBundleToCartWithDiscount**_ API call
3. No hard coding of Product Ids. If we do, then the **$Hash** value verification will fail because it uses Product Ids returned from recommendation
4. The sample below has comments highlighting these points

<details>

<summary>Dynamic Bundle recommendation template sample</summary>

```velocity
<!--

Nosto Solutions Oy

User will find comments along the document which can be helpful
to tweak it and make it bespoke to different eCommerces' look&feel.

-->
  
#if($products.size() > 0)
#set($imgCount = 0)

<div class="nosto-block">
  <span class="nosto-header">$!title</span>
  <ul class="nosto-list nosto-carousel">
    #foreach($product in $products)
        <li class="nosto-list-item nosto-product selected" nosto-price="$!product.price.asNumber()" nosto-handle="$!product.lastPathOfProductUrl()" nosto-id="$!product.productId" >
          <div class="nosto-image-container img-$imgCount"></div>
          <div class="nosto-product-info">
            <a href="$!product.url" class="nosto-product-name">$!product.name.truncate(100)</a>
            <div class="bottom-info">
                <a href="$!product.url" class="nosto-product-price">
                  <span class="nosto-oldprice">$!product.listPrice</span>
                  <span class="nosto-newprice">$!product.price</span>
                </a>
                <div class="control tocart-qty">
                    <span class="minus">-</span>
                    <input type="number" name="qty" class="input-text shadow qty" min="0" maxlength="12" pattern="\d*" value="1" title="Qty">
                    <span class="plus">+</span>
                </div>
            </div>
          </div>
        </li>
        #set($imgCount = $imgCount + 1)
        #if($!foreach.count < $!products.size())
            <li class="nosto-plus #if($!foreach.count % 2 == 0) nosto-plus-even #end" style="border: none;">+</li>
        #end
    #end
    <li class="nosto-list-item nosto-actions" style="border: none;">
        <div class="price-box price-final_price" data-role="priceBox">
            <span style="margin-right: 10px;font-weight:bold;font-size: 1.5em;">Total</span>
            <span class="price-tag  color1">
              <span class="price-container price-final_price tax weee" itemprop="offers" itemscope="" itemtype="http://schema.org/Offer">
                 <div class="nosto-total">
                    <span class="nosto-product-price" data-price-type="finalPrice"></span>
                    </div>
                    <div class="nosto-discounted-total">
                        <span class="nosto-product-price" data-price-type="finalDiscountedPrice">&nbsp;</span>
                    </div>
                 </span>   
                 <meta itemprop="nosto-product-price" content="1">
                 <meta itemprop="priceCurrency" content="$!product.currencyCode">
              </span>
            </span>
        </div>
        <!--span class="nosto-total"></span-->
        <button class="action primary tocart button button--large button--full color " id="nosto-atc">ADD TO CART</button>
    </li>
  </ul>
</div>
#end
  
<script type="text/javascript" charset="utf-8">

    (function() {
        
        const twd = _targetWindow.document;
        const currencySymbol = "$!products[0].currencySymbol";
        
        const discountType = "$!props.discountType";
        const discountValue = #if($!props.discountValue && $!props.discountValue != '') + "$!props.discountValue" #else 0 #end;

        //SELECT ITEMS
        const productCount = twd.querySelectorAll('#$divId .nosto-product').length;
        
        let processedCount = 0;
        twd.querySelectorAll('#$divId .nosto-product').forEach((element, index) => {

            let handle = element.getAttribute('nosto-handle');

            request({url: '/products/' + handle + '.js'}).then(data => {
                element.setAttribute('nosto-sku-id', data.variants[0].id);
                element.setAttribute('nosto-sku-price', convertToDecimal(data.variants[0].price));
                processedCount++;
                if (processedCount === productCount) {
                    updateTotal();
                }
            });
        })

        //REFER THIS SECTION FOR THE addBundleToCartWithDiscount API CALL
        twd.querySelector('#$divId #nosto-atc').addEventListener('click', function(e) {
            
            e.stopPropagation();

            const sourceElement = e.srcElement ? e.srcElement : e.target
            const allBundleProductIds = [];

            twd.querySelectorAll('#$divId .nosto-product').forEach(function(element, index) {
                allBundleProductIds.push(element.getAttribute('nosto-id'));
            })

            const bundleItemsSelected = [];

            // FIRST ADDING PRODUCT IDS OF ALL PRODUCTS RETURNED FROM THE RECOMMENDATION
            twd.querySelectorAll('#$divId .nosto-product.selected').forEach(function(element, index) {
                bundleItemsSelected.push({
                    'productId': element.getAttribute('nosto-id'),
                    'variantId': element.getAttribute('nosto-sku-id'),
                    'quantity': parseInt(element.querySelector('.tocart-qty input.qty').value)
                })
            })
            
            // ADDING CURRENTLY VIEWED PRODUCT DETAILS TOWARDS THE END
            if ("$!props.discountCurrentProduct" === "true") {
                allBundleProductIds.push("$!context.viewedProduct.productId")
                bundleItemsSelected.push({
                    'productId': "$!context.viewedProduct.productId",
                    'variantId': "$!context.viewedProduct.skus[0].id",
                    'quantity': 1
                })
            }

            _targetWindow.Nosto.addBundleToCartWithDiscount({
                items: bundleItemsSelected,
                discount: {
                    type: discountType,
                    value: discountValue
                },
                bundleProducts: allBundleProductIds,
                hash: "$hash"
            }, this).then(() => console.log("addBundleToCartWithDiscount completed"))

        })
        
        twd.querySelectorAll("#$divId .tocart-qty").forEach(function(element) {
          
          element.querySelector(".minus").addEventListener("click", function(e){
              e.stopPropagation();
              const sourceElement = e.srcElement ? e.srcElement : e.target
              const qtyElement = sourceElement.parentElement.querySelector(".qty");
              const value = parseInt(qtyElement.value) - 1;
              if (value < 1) {
                  qtyElement.value = 1;
              } else {
                  qtyElement.value = value;
                  updateTotal();
              }
          })
          
          element.querySelector(".plus").addEventListener("click", function(e){
              const sourceElement = e.srcElement ? e.srcElement : e.target
              const qtyElement = sourceElement.parentElement.querySelector(".qty");
              qtyElement.value = parseInt(qtyElement.value) + 1;
              e.stopPropagation();
              updateTotal();
          })
        });
        
        
        /* Functions */
        
        function convertToDecimal(price) {
            const priceStr = price.toString();
            const size = priceStr.length;
            const result = priceStr.slice(0, size - 2) + "." + priceStr.slice(size - 2);
            return Number(result).toFixed(2);
        }
        
        function updateTotal() {
            let total = 0;
            let discounted = 0;
            twd.querySelectorAll("#$divId .nosto-product.selected").forEach(function(element) {
                total += parseFloat(element.getAttribute("nosto-sku-price")) * parseFloat(element.querySelector('.tocart-qty input.qty').value);
            })
            
            if (discountType == 'percent') {
                discounted = total - (total / 100 * discountValue);
            } else {
                discounted = total - (productCount * discountValue);
            }
            
            total = total.toFixed(2).toString();
            twd.querySelector("#$divId .nosto-total .nosto-product-price").innerHTML = currencySymbol + total;
            
            discounted = discounted.toFixed(2).toString();
            
            if (discounted === total || total === 0.00) {
                twd.querySelector('#$divId .nosto-discounted-total').style.display = 'none';
                twd.querySelector('#$divId .nosto-total').classList.remove('content-striked');
            } else {
                twd.querySelector('#$divId .nosto-discounted-total').style.display = '';
                twd.querySelector('#$divId .nosto-total').classList.add('content-striked');
                twd.querySelector("#$divId .nosto-discounted-total .nosto-product-price").innerHTML = currencySymbol + discounted;
            }
            
        }
        
        function request(obj) {
            return new Promise((resolve, reject) => {
                let xhr = new XMLHttpRequest();
                xhr.open(obj.method || "GET", obj.url, true);
                xhr.setRequestHeader("Content-type", "application/json");
                if (obj.headers) {
                    Object.keys(obj.headers).forEach(key => {
                        xhr.setRequestHeader(key, obj.headers[key]);
                    });
                }
                xhr.onload = () => {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        resolve(JSON.parse(xhr.responseText));
                    } else {
                        reject(xhr.statusText);
                    }
                };
                xhr.onerror = () => reject(xhr.statusText);
                obj.body ? xhr.send(JSON.stringify(obj.body)) : xhr.send();
            });
        };
        
    })();

</script>
```

</details>

## Troubleshooting

### Issue #1: Additional content displayed on cart page

![Properties Exposed](https://user-images.githubusercontent.com/82023195/168611801-666c24bc-c7f9-4edd-bdf5-2be32798391c.png)

### Cause

Nosto implementation uses a private property field for sharing discount information with Shopify. In newer theme versions, Shopify automatically hides these fields while displaying products in cart page. In older version of themes, these fields gets exposed as shown below.

### Solution

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

### Issue #2: Discount is not applied

### Cause

1. **Nosto provided line item script is customized** (_No support provided_).
2. SECRET\_KEY value is incorrect.
3. Issue with Bundle script configuration.
4. Not all the products from a bundle are added to cart.

### Solution

Contact Nosto support and share the line item script content for further analysis.
