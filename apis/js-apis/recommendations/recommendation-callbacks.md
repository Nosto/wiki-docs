# Recommendation Callbacks

The JS API can be used to register callbacks to hook into the recommendation events. To register a listener for a callback, use `api.listen(callbackId, callbackFunction)` function.

### Post Render Callback <a href="#post-render-callback" id="post-render-callback"></a>

Called when Nosto has responded and finished recommendation placement.

{% code overflow="wrap" %}
```javascript
nostojs(api => {
  api.listen('postrender', event => {
    console.log(event.filledElements);
    console.log(event.unFilledElements); 
  });
});
```
{% endcode %}

#### Fields <a href="#fields" id="fields"></a>

|                  |       |                                                                                 |
| ---------------- | ----- | ------------------------------------------------------------------------------- |
| filledElements   | Array | Contains a list of recommendation slots that contain recommendations            |
| unFilledElements | Array | Contains a list of recommendations slots that did not have get recommendations. |

### Pre Render Callback <a href="#pre-render-callback" id="pre-render-callback"></a>

Called when Nosto has responded but not yet rendered content. The event contains information about the visitor's preferences.

{% code overflow="wrap" %}
```javascript
nostojs(api => {  
  api.listen('prerender', event => {    
    console.log(event);
  });
});
```
{% endcode %}

#### Fields <a href="#fields-1" id="fields-1"></a>

<table data-header-hidden><thead><tr><th></th><th width="115.0859375"></th><th></th></tr></thead><tbody><tr><td></td><td></td><td></td></tr><tr><td>affinityScores</td><td>Object</td><td><p>Describes visitor's most preferred brands and categories, the object has two attributes top_brands and top_categories which are arrays that contain maximum 5 most preferred brands/categories for the visitor. Example content:</p><pre class="language-json" data-overflow="wrap"><code class="lang-json">{
top_brands: [
{
    name:"Acme",
    score:0.8
},
{
    name:"Universal",
    score:0.3
}]
}
</code></pre></td></tr><tr><td>segments</td><td>Object</td><td><p>Information about which segments the visitor falls into. active_segments field contains a list of segment identifiers that were active on the latest request to Nosto. Segment identifiers can be retrieved from the Nosto backend, or queried from the <a href="https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-queries/graphql-querying-segments">GraphQL API</a>. Example content:</p><pre class="language-json"><code class="lang-json">{
active_segments:[
{
id:"5a497a000000000000000004"
},
{
id:"5aa12b8960b2352d326d77f1"
}]
}
</code></pre></td></tr></tbody></table>

For more information, please refer to the [event-api-listening-to-bus-events-with-api.listen.md](../../../implementing-nosto/implement-on-your-website/advanced-implementation/event-api-listening-to-bus-events-with-api.listen.md "mention")
