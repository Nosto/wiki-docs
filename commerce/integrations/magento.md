# Magento

* [Product Page Widgets](magento.md#pdpwidget)

## Product Page Widgets

To embed UGC Widgets into a specific Product Page, and have the Widget dynamically update based upon the Product Page it has been added, you will need to edit the Product Page template within your Magento instance.

The file you will generally wish to update is titled `details.phtml` and is generally located under `/design/frontend///Magento_Catalog/templates/product/view`.

Within this file you will want to add in the Nosto's UGC Embed code for a Widget from your Stack, plus the variable `data-tags: ext:<?php echo $tagSku; ?>`.

The embedded code should look something like this:

```javascript
<div class='stackla-widget' data-id='' data-hash='' data-filter='' data-tags: ext:<?php echo $tagSku; ?> data-ct='' data-alias='client.stackla.com' data-ttl="30" ></div>
<script type='text/javascript'>
    (function (d, id) {
        if (d.getElementById(id)) return;
        var t = d.createElement('script');
        t.type = 'text/javascript';
        t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js';
        t.id = id;
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t);
    }(document, 'stacklafw-js'));
</script>
```

The client should embed code wherever in the page they would like the Widget to render.

The variable `data-tags: ext:<?php echo $tagSku; ?>` will tell the Widget to find all content which contains the Product SKU from Magento.

[Return to Top](magento.md#top)
