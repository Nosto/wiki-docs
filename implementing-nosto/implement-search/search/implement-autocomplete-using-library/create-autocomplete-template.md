# Create Autocomplete template

## Attributes

The following `data-*`  attributes are required by the library to handle attributions (click events) for products/keywords/history items rendered in the autocomplete result:

`data-ns-hit`

This attribute should be used on clickable `keyword`, `product`, `history` list elements. This attribute handles submit keyword/history as search, redirect to product, analytics (if enabled) request.

{% hint style="info" %}
### Encode HTML content

**This is specific to cases where no template language like liquid/handlebars is used and the content is rendered using plain HTML.**

Make sure to HTML encode content passed to this attribute. Bacause `JSON.stringify` may produce result that can't be directly used in HTML especially when the content includes special characters.

For example, consider the below example

```json
{
  "keyword": "new year's eve",
  "_highlight": { "keyword": "new year's eve" }
}
```

can be encoded as

{% code overflow="wrap" %}
```
{&quot;keyword&quot;:&quot;year's eve&quot;,&quot;_highlight&quot;:{&quot;keyword&quot;:&quot;<strong>year</strong>'s eve&quot;}}
```
{% endcode %}
{% endhint %}

Following table shows value for this attribute depending on the rendering context.

<table><thead><tr><th width="100">Context</th><th width="686.50390625">Value</th></tr></thead><tbody><tr><td>keyword</td><td><p>value from <code>response.data.search.keywords</code></p><p></p><p>Code example:</p><pre class="language-javascript"><code class="lang-javascript">const { keywords } = response.data.search
const contentToRender = keywords.map(keyword => 
    `
    &#x3C;div data-ns-hit="${JSON.stringify(keyword)}" ....>
        ....
        ....
    &#x3C;/div>
    `
)
</code></pre><p>Value example:</p><pre class="language-json"><code class="lang-json">{
    "keyword": "midi dresses",
    "_highlight": { "keyword": "midi &#x3C;strong>dress&#x3C;/strong>es" }
}
</code></pre></td></tr><tr><td>product</td><td><p>productId and url from <code>response.data.search.products.hits</code></p><p></p><p>Code example:</p><pre class="language-javascript"><code class="lang-javascript">const { hits } = response.data.search.products
const contentToRender = hits.map(({ productId, url }) => 
    `
    &#x3C;div data-ns-hit="${JSON.stringify({ productId, url })}" ....>
        ....
        ....
    &#x3C;/div>
    `
)
</code></pre><p>Value example:</p><pre class="language-json"><code class="lang-json">{
    "productId": 123456,
    "url": "https://example.com/products/example-product-handle"
}
</code></pre></td></tr><tr><td>history</td><td>historyEnabled &#x26; historySize config. Refer <a href="initialization/">Initialization</a></td></tr></tbody></table>

`data-ns-remove-history`

This attribute should be used to delete history entries in the autocomplete

To make an element delete a single history entry when clicked, add `data-ns-remove-history={hit.item}` to an element. In order to delete all history entries, add `data-ns-remove-history="all"` to clear button.

## Starter templates

This section provides links to default startup templates for different rendering frameworks. These templates can be copied and customized as needed.&#x20;

[Handlebars](https://github.com/Nosto/nosto-autocomplete/blob/main/src/handlebars/autocomplete.handlebars), [Mustache](https://github.com/Nosto/nosto-autocomplete/blob/main/src/mustache/autocomplete.mustache), [Liquid](https://github.com/Nosto/nosto-autocomplete/blob/main/src/liquid/autocomplete.liquid), [React/Preact (HTML)](https://github.com/Nosto/nosto-autocomplete/blob/main/src/react/Autocomplete.tsx)

{% hint style="info" %}
#### Mustache helpers

_**Mustache is based on logic-less templates which can be enhanced with helpers, e.g `toJson`, `imagePlaceholder`, `showListPrice` in example template**_.

```javascript
import { fromMustacheTemplate } from '@nosto/autocomplete/mustache'

fromMustacheTemplate(template, {
    helpers: {
        toJson: function () {
            return JSON.stringify(this)
        },
    },
})
```
{% endhint %}
