# Guides

To demonstrate and help you get started, the following are some examples of common uses of Nosto's UGC to enrich an existing website or application.

#### Note about guide formatting

The structure of the guides is mostly these three parts:&#x20;

1. Introduction to the use case
2. In-depth code or analysis
3. Next steps or further exercises

In addition, we've capitalized words when they refer to specific Nosto's UGC concepts to make them stand out more. For example, the word Tag is capitalized when talking about a UGC Tag, as opposed to a tag in the context of clothing, a tag in the context of word clouds, or a tag in the context of graffiti.

```javascript
<script type="text/javascript">(function (d, id) { 
    var t, el = d.scripts[d.scripts.length - 1].previousElementSibling; 
    if (el) el.dataset.initTimestamp = (new Date()).getTime(); 
    if (d.getElementById(id)) 
        return; t = d.createElement('script'); 
        t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js'; 
        t.id = id; (d.getElementsByTagName('head')[0] || 
        d.getElementsByTagName('body')[0]).appendChild(t); 
    }(document, 'stackla-widget-js'));
</script>
```
