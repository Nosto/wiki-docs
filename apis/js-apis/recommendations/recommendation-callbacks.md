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

| Field | Type | Reason |
| :--- | :--- | :--- |
| filledElements | Array | Contains a list of recommendation slots that contain recommendations |
| unFilledElements | Array | Contains a list of recommendations slots that did not have get recommendations. |

## Pre Render Callback <a id="pre-render-callback"></a>

Called when Nosto has responded but not yet rendered content. The event contains information about the visitor's preferences. 

```javascript
nostojs(api => {  
  api.listen('prerender', event => {    
    console.log(event);
  });
});
```

### Fields <a id="fields"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Reason</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">affinityScores</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left">
        <p>Describes visitor&apos;s most preferred brands and categories, the object
          has two attributes top_brands and top_categories which are arrays that
          contain maximum 5 most preferred brands/categories for the visitor. Example
          content:</p>
        <p><code>{</code>
        </p>
        <p><code>  top_brands:[ </code>
        </p>
        <p><code>    {name:&quot;Acme&quot;, score:0.8}, </code>
        </p>
        <p><code>    {name:&quot;Universal&quot;, score:0.3}</code>
        </p>
        <p><code>  ]</code>
        </p>
        <p><code>}</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">segments</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left">
        <p>Information about which segments the visitor falls into. active_segments
          field contains a list of segment identifiers that were active on the latest
          request to Nosto. Segment identifiers can be retrieved from the Nosto backend,
          or queried from the <a href="https://docs.nosto.com/techdocs/apis/graphql-an-introduction/graphql-using-queries/graphql-querying-segments">GraphQL API</a>.
          <br
          />
          <br />Example content:</p>
        <p><code>{</code>
        </p>
        <p><code>  active_segments:[ </code>
        </p>
        <p><code>    {id:&quot;5a497a000000000000000004&quot;}, </code>
        </p>
        <p><code>    {id:&quot;5aa12b8960b2352d326d77f1&quot;}</code>
        </p>
        <p><code>  ]</code>
        </p>
        <p><code>}</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

