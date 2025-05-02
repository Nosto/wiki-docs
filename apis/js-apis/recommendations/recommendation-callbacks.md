# Recommendation Callbacks

The JS API can be used to register callbacks to hook into the recommendation events. To register a listener for a callback, use `api.listen(callbackId, callbackFunction)` function.

## Post Render Callback

Called when Nosto has responded and finished recommendation placement.

```javascript
nostojs(api => {
  api.listen('postrender', event => {
    console.log(event.filledElements);
    console.log(event.unFilledElements); 
  });
});
```

### Fields

| Field            | Type  | Reason                                                                          |
| ---------------- | ----- | ------------------------------------------------------------------------------- |
| filledElements   | Array | Contains a list of recommendation slots that contain recommendations            |
| unFilledElements | Array | Contains a list of recommendations slots that did not have get recommendations. |

## Pre Render Callback <a href="#pre-render-callback" id="pre-render-callback"></a>

Called when Nosto has responded but not yet rendered content. The event contains information about the visitor's preferences.

```javascript
nostojs(api => {  
  api.listen('prerender', event => {    
    console.log(event);
  });
});
```

### Fields <a href="#fields" id="fields"></a>

| Field          | Type   | Reason                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| -------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| affinityScores | Object | <p>Describes visitor's most preferred brands and categories, the object has two attributes top_brands and top_categories which are arrays that contain maximum 5 most preferred brands/categories for the visitor. Example content:</p><p><code>{</code></p><p><code>top_brands:[</code></p><p><code>{name:"Acme", score:0.8},</code></p><p><code>{name:"Universal", score:0.3}</code></p><p><code>]</code></p><p><code>}</code></p>                                                                                                                                                                                                      |
| segments       | Object | <p>Information about which segments the visitor falls into. active_segments field contains a list of segment identifiers that were active on the latest request to Nosto. Segment identifiers can be retrieved from the Nosto backend, or queried from the <a href="https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-queries/graphql-querying-segments">GraphQL API</a>.<br><br>Example content:</p><p><code>{</code></p><p><code>active_segments:[</code></p><p><code>{id:"5a497a000000000000000004"},</code></p><p><code>{id:"5aa12b8960b2352d326d77f1"}</code></p><p><code>]</code></p><p><code>}</code></p> |
|                |        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
