# Basic Tagging

In the event that you are unable to expose the entire subset of the product tagging, you can simply tag the product-id.

**Note:** The product tagging _must_ be server-side rendered as the Nosto crawler does not execute Javascript.

```markup
<div class="nosto_page_type" style="display:none">product</div>
<div class="nosto_product nosto_basic" style="display:none"> 
  <span class="product_id">Canoe123</span>
</div>
```

When the entirety of the product metadata is tagged, Nosto is able to crawl your site and build a 1:1 replica of your product catalog but in this basic example, you will need to use an alternative mechanism for synchronising your catalog with Nosto.

**Note:** If you do use this approach, your account-manager must disable crawling for your account. Failure to do so will result in a broken catalog replica.

### Via an API

In order to keep your product catalog in Nosto up to date, you must leverage the [Products API](../../../../apis/rest/products/updating-products-using-the-products-api.md).

### Via a Feed

Nosto does not support a product feed and you must leverage the API in order to synchronise your product catalog.

## Troubleshooting

Once included on all pages, you can review if the site is transmitting data using the [Nosto Debug Toolbar](https://help.nosto.com/get-started/guides/how-to-use-the-nosto-debug-toolbar). If you can see product attributes being picked up under "Tagging" then the product details are correctly set up. You can further verify that products are being indexed to the catalog under the Nosto admin by navigating to Tools → Products: [https://my.nosto.com/admin/$accountID/campaigns/products/list](https://my.nosto.com/admin/$accountID/campaigns/products/list)

![Nosto debug toolbar / products](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-product-tagging.png)

![Nosto product catalogue](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-product-catalogue.png)

![live-feed-product-view](https://nosto-campaign-assets.s3.amazonaws.com/images/live-feed-view.png)

