---
description: >-
  Nosto’s Recommendation Extensions allow you to personalize your customers’
  journey directly inside the Shopify Checkout and Thank You pages.
---

# Recommendation Extensions

## Overview

Extensions are added as native app blocks via Shopify’s Checkout Editor and render personalized product recommendations at key moments — either before the order is confirmed, or right after.

The Recommendation Extensions are available for:

### Checkout page (Shopify Plus only)

→ Shopify's Checkout page, where customer enters payment details & shipping information

### Thank You page

→ Appears after Checkout is done, confirming the order

Each extension shows up as a Nosto app block and supports standard Shopify layout controls.

## Requirements

To use the extensions, you’ll need:

* The Nosto app installed from the Shopify App Store
* Product Recommendations enabled in your Nosto account
* Nosto's Shopify Pixel enabled - [find out more](https://docs.nosto.com/shopify/tracking-and-session-management)
* Shopify Plus (for Checkout Extension support)
* Access to the Checkout Editor in your Shopify Admin

Once these are in place, you can add and customize the Nosto blocks directly.

## How to Set Up in Nosto

Before setting the Extensions up in Shopify, ensure they are set and enabled correctly in Nosto.&#x20;

To ensure the Recommendation Extensions always follow your desired strategy, adjust your Recommendation logics accordingly. This can be done in your Nosto account, and follows the general rules and possibilities of Product Recommendation.&#x20;

You can find your Extension Recommendations already pre-set under _Nosto Admin → Product Recommendations,_ listed under page types "Shopify Checkout Page" & "Shopify Thank You Page".

## How to Set Up in Shopify

### 1. Open the Checkout Editor

* Go to _Shopify Admin → Settings → Checkout_
* Click “**Customize**” to open the editor

### 2. Add Nosto app blocks

* Under _Apps_, ensure your desired Nosto Extension is enabled
* Under _Checkout → Sections_, click "_Add App block_" and add "Nosto Checkout" or "Nosto Thank-You"

In the editor sidebar, you can adjust the position within the allowed areas, like below the order summary or beside shipping options.

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption><p>Example for positioning of Nosto Checkout block</p></figcaption></figure>

## Visual Customization

Nosto Recommendation Extensions will automatically adjust their design, based on your Checkout theme settings and styling. However, Nosto extensions support a few flexible appearance settings that let you customize how content is displayed on your checkout page.&#x20;

These settings appear within your Theme Editor, when selecting the Nosto Extension.&#x20;

### Available options

<table><thead><tr><th>Setting</th><th width="208.92578125">What it does</th><th>Values</th></tr></thead><tbody><tr><td><strong>Aspect Ratio</strong></td><td>Sets the image ratio</td><td>1, 1.5, 0.75</td></tr><tr><td>I<strong>mage Fit</strong></td><td>Controls image scaling</td><td>cover, contain, fill</td></tr><tr><td><strong>Product Title Font Size</strong></td><td>Adjusts text size of the product title</td><td>small, medium, large</td></tr><tr><td><strong>Strike-Through Price</strong></td><td>Allows control over showing or hiding the strike-through price if a product is discounted. </td><td><p><code>true =</code> €478.00 </p><p><code>false =</code> <del>€666.00</del> €478.00</p></td></tr><tr><td><strong>Button Style</strong></td><td>Gives control over the styling of the ATC button. </td><td>base-primary, monochrome-primary, base-secondary, critical-secondary, monochrome-secondary</td></tr><tr><td><strong>New Recommendation Design</strong></td><td>Allows switching between two designs for your Recommendations. New design is used by default for mobile.  </td><td>true, false</td></tr><tr><td><strong>Show Description</strong></td><td>Show or hide the product description (new design only)</td><td>true, false</td></tr></tbody></table>

{% hint style="info" %}
Some of these options are only available for the **Checkout Extension**, not for the Thank You Extension.
{% endhint %}

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Extension - Visual Customization</p></figcaption></figure>

## Known Limitations

| **Checkout Extension** | Requires Shopify Plus                                        |
| ---------------------- | ------------------------------------------------------------ |
| **Max Products**       | Only 2 products will be shown per block                      |
| **Pixel dependency**   | The Nosto Pixel must be active and session cookie present    |
| **Cookie Consent**     | If customer declines tracking, recommendations will not load |



If unsure, please reach out to your Nosto team via support@nosto.com!

