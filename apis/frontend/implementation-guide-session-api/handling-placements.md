# Handling Placements

Onsite features such content and recommendations are injected into what we call "placements".

[Placements - General Article](https://help.nosto.com/en/articles/1883767-placements-general-article)

Nosto requires that you specify an array of placements and will respond with the response for those specified placements. There are two types of placements and it is possible to leverage either of them when implementing Nosto on an SPA.

#### Static Placements

These are defined as empty hidden elements in the DOM with the class `nosto_element`. The identifier of the element denotes the placement identifier. An example placement is as follows:

```text
<div class="nosto_element" id="home-1" style="display: none;"></div>
```

The DIV remains hidden until Nosto injects content into it.

#### Dynamic Placements

These are defined in the administration panel and are CSS and URL rules to define where content is to be injected.

## Managing Placements Manually

If you require complete control over which placements are requested and how they are templated and how they are injected, you can do so by using our JS API.

```text
nostojs(api ⇒ api.defaultSession()
  .setElements(['front-bestsellers-1'])
  .setResponseMode('JSON')
  .load()
  .then(response => response.recommendations)
```

This example allows you to specify what placements are fetched. The above example will fetch the content for the campaign associated with `front-bestsellers-1`.

For legacy reasons, the term "placements" and "elements" are often used interchangeably but refer to the same thing. Notice the method in the example above is called `setElements` instead of `setPlacements`

The response object will contain an object where the keys are the placement identifiers and the value will be an object containing the payload.

This example assumes that you will deduce what placements are to be requested and that the response for the placements will be in a mixed JSON and HTML format. Any recommendations that are requested will have a JSON response, while any content that is requested will have and HTML response.

While the API method is termed as `setResponseMode`, it simply flags the "preferred" response format. All recommendations in the response will be in JSON, while all content will be in HTML.

In this scenario, Nosto does not do any templating. The responsibility of templating the response and injecting it is up to the implementation.

Here is an example of a sample response.

## Managing Placements Automatically

In the event that you would like to offload the campaign rendering to Nosto, you can use the Placements API to inject the resultant campaigns.

```text
nostojs(api ⇒ api.defaultSession()
  .setElements(api.placements.getPlacements())
  .setResponseMode('HTML')
  .load()
  .then(response => {
    api.placements.injectCampaigns(response)
  })
```

The example above will fetch the data associated with campaign `front-bestsellers-1` and inject it into the associated elements in the DOM.

Although is possible to change the value of the `setResponseMode` to `JSON`, this would be incorrect. Doing so will cause the `injectCampaigns` invocation to inject a blob of JSON into the page. We recommend that you always use `HTML` as a parameter of the`setResponseMode` method.

The `injectCampaigns` method automatically handles the injection of both the recommendations and the content. If there is no response for a specified placement, the element \(into which to be injected\), remains as-is.

