# Getting Started

Nosto's UGC provides several ways to track the impact of your UGC initiatives within your Google Analytics instance. This article will instruct you on how to configure the Google Analytics 4 plugin for it.

### STEP 1: Install Google Analytics 4 on your website

To use the Google Analytics 4 plugin, you will need Google Analytics installed on the pages you wish to track.

Please follow the instructions here to prepare your gtag tracking code: [https://developers.google.com/tag-platform/gtagjs/install](https://developers.google.com/tag-platform/gtagjs/install)

The gtag is essential for the integration to work, as it is used to send events to Google Analytics 4.

For commerce customers, we can also attribute UGC to E-commerce transactions by utilizing the gtag on your website.

### STEP 2: Install the UGC Commerce Script & Nosto tagging on your website

**If your widgets are deployed via** [**Nosto Placements**](https://app.intercom.com/a/apps/zmcdradn/articles/articles/1883767/show) **please skip step 2.**

**If your widgets are not deployed via** [**Nosto Placements**](https://app.intercom.com/a/apps/zmcdradn/articles/articles/1883767/show)**, please follow the steps below to configure UGC Commerce tracking for your website.**

Note: This step can also be completed after Step 3.

#### 2.1 Install the UGC Commerce script

This script is utilized to attribute conversions to your UGC widgets by sending properties along with commerce events.

For example, if a user clicks on a product within a UGC widget, we will send the product information to Google Analytics 4 when the user clicks an add-to-cart event, product view event, or purchase event.

The script can be installed by adding the following code to the header of your website:

```html
<script src="https://my.stackla.com/media/js/dist/ugc.bundle.js"></script>
```

#### 2.2 Install Nosto Product Tagging

**If you already have the Nosto Product tagging on your website via the Nosto Personalization solution please skip this step**

For pages that relate to one product, for example, a product view page - we require the following:

```html
  <div class="nosto_page_type" style="display:none">product</div>
  <div class="nosto_product" style="display:none"> 
    <span class="product_id">{{ product.id }}</span>
  </div>
```

Please note, that you will be required to add the product id to the product\_id span.

This can be rendered using your backend, before the page loads. Or, if using an e-commerce system like Shopify, you can utilize a liquid variable to preload the product ID.

#### 2.3. Install Nosto Cart Tagging

**If you have the Nosto Cart tagging on your website already via the Nosto Personalization solution please skip this step**

For all pages, we recommend the utilization of our cart tagging.

This will allow Nosto's UGC to track the products which are added to the cart and purchased, to increase the accuracy of UGC Commerce tracking for your organization.

```
<div class="nosto_cart" style="display:none">
    <div class="line_item">
        <span class="product_id"></span>
    </div>
</div>
```

The product ID span will need to be populated with the product ID of the product that is added to the cart.

For example, by using **Shopify liquid** this can be done with the following code:

```
<div class="nosto_cart" style="display:none">
    <div data-gb-custom-block data-tag="for">

    <div class="line_item">
        <span class="product_id">{{ item.product.id }}</span>
    </div>
    

</div>
</div>
```

**Please paste this code into your code's main theme file** so the UGC Commerce script can access it.

The body of the code is a good place to keep it.

Learn more about Commerce Events Tracking [here](https://developer.stackla.com/analytics/google-analytics-v4/commerce/).

### STEP 3: Connect the Google Analytics 4 Plugin

To track UGC events in Google Analytics 4 you will need to configure the Google Analytics 4 plugin in the Nosto Admin Portal. Please follow the steps here: https://help.nosto.com/en/articles/7890296-google-analytics-4-authentication-configuration

Once the steps above are completed your Performance events will start tracking automatically. Learn more about Performance Event Tracking [here](https://developer.stackla.com/analytics/google-analytics-v4/events/).

If you want to track Email Campaigns you will also have to complete step 4.

### STEP 4: Embed your Email Campaigns

Once you have completed the Google Analytics 4 plugin configuration and enabled the Email tracking, you will need to go to the Email campaigns and re-embed the Tiles via link or HTML. You will notice that both the link and HTML include a piece of tracking code that is used to track email events.

If you select to track Email Clicks, the embeds will also automatically include the link to the original social post.

Learn more about how to embed Email Campaigns [here](https://help.nosto.com/en/articles/5793357-how-to-create-an-email-campaign).

Learn more about Email Events Tracking [here](https://developer.stackla.com/analytics/google-analytics-v4/email/).

***

Once all the steps above are completed, you will be able to track all Performance, Email, and Commerce events for your UGC initiatives within Google Analytics 4.

Please note the Google Analytics data provided through the Google Reporting API is not real-time so, **it may take 24-48 hours (after an event is tracked) for data to appear** in the Performance, Commerce, and Email Dashboards in the Nosto Admin Portal.

[Back to Top](getting-started.md#top)
