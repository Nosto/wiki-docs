# Handling Placements

Onsite features such content and recommendations are injected into what we call "placements".

[Placements - General Article](https://help.nosto.com/en/articles/1883767-placements-general-article)

Nosto requires that you specify an array of placements and will respond with the response for those specified placements. There are two types of placements and it is possible to leverage either of them when implementing Nosto on an SPA:

### Static Placements

These are defined as empty hidden elements in the DOM with the class `nosto_element`. The identifier of the element denotes the placement identifier. An example placement is as follows:

```
<div class="nosto_element" id="home-1" style="display: none;"></div>
```

The DIV remains hidden until Nosto injects content into it.

### Dynamic Placements

These are defined in the administration panel and are CSS and URL rules to define where content is to be injected. In order to allow these dynamic placements to be usable, you should follow the guide below for managing placements automatically.

## Managing Placements Automatically

When using the Session API, the placements are set through a function call `setPlacements` which expects an array of strings. In order to support dynamic placements setup in the Nosto backend, we recommend that instead of setting the placements array contents directly from your application, you use the following API to get a list of placements for the page:

```
api.placements.getPlacements()
```
The function scans the current page (document object) for any valid placements at the time of execution and returns them in an array suitable for the `setPlacements`-function. As long as it is called after the targeted elements have been rendered into the document, it will return both the static and the dynamic CSS and URL rules based placements on the page. Dynamic placements are picked up regardless of them being rendered or not. Static placements are always obtained through scanning the page. Freshly created Nosto accounts come with static placements only. ( In case you have programmatic placement codes that are not in the page document at all, you can add them to the array to combine both manual and automatic approaches. )

Here's an example call where placements are scanned automatically:

```
nostojs(api => {
  api.defaultSession()
    .viewFrontPage()
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => console.log(response.campaigns))
});
```

To continue with the example, let's assume the `api.placements.getPlacements()` returned an array with 2 placements:` ['frontpage-center-1', 'frontpage-banner']` and then let's assume that we would have setup in the backend a recommendation campaign for `frontpage-center-1` and a content campaign for `frontpage-banner`.&#x20;

Here is an example of a sample response.

```
{ 
    "campaigns": {
        "content": {
            "frontpage-banner": {
                div_id: "frontpage-banner",
                result_id: "5fc6390c60b2ecd3cc0c2d4f",
                html: "< Campaign html content >",
                params
            }
        },
        "recommendations": {
            "frontpage-center-1": {
                div_id: "frontpage-center-1",
                result_id: "frontpage-center-1",
                products: [ /* Array of products */ ],
                params
            }
        }
    }
}
```

The response object will contain a campaigns field which has both content and recommendations under their own fields and within those the keys are the placement identifiers and the value will be an object containing the payload and parameters used for click attribution.&#x20;

## Rendering campaign results

By default the API returns campaigns data in JSON notation. Product campaigns contain an array of recommended products and content campaigns contain an html field. The implementing application can decide how it does the rendering in the application, but Nosto does provide a utility function to help with injecting the html content campaigns into the right place: `api.placements.injectCampaigns()`. The function expects an object where the field keys are the placements to be injected and values are either a string or an object with a string field named `html`. The function will scan the document to find the active placements and insert the html to the right location. Any javascript blocks within the html content will be executed as well.

Here is an example of rendering campaign results.&#x20;

```
/* TODO for the application to implement */
function createProductRecsHtml(recommendations){
  return new Promise((resolve, reject) => {
    /* TODO your code to create the html for each recommendation 
     * with the placements identifiers as keys, example:
     * recsHtml = { "frontpage-center-1":"html here" }
     */ 
    resolve(recsHtml);
  }
}

nostojs(api => {
  api.defaultSession()
    .viewFrontPage()
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => { 
      /* Render content campaigns */ 
      api.placements.injectCampaigns(response.campaigns.content);
      
      /* Transform products json to html and render */
      createProductRecsHtml(response.campaigns.recommendations).then(recsHtml => {
        api.placements.injectCampaigns(recsHtml);  
      });
    })
});
```

This example assumes the implementing application has a utility function to transform products json into html to be inserted, alternatively there could also be a function that gets the product json and renders them itself directly.

## Offloading campaign rendering fully to Nosto&#x20;

In the event that you would like to offload the campaign rendering fully to Nosto, you can skip transforming the products json to html and instead use the recommendation templates in the Nosto backend to produce html. In that case you would set the response mode in the Session API to be 'HTML', then the response will not have a `campaign` field and will instead contain a `recommendations` field which has the HTML content of all campaigns, regardless if they are content or recommendation campaigns. You can then pass that field to a utility method `api.placements.injectCampaigns` that will inject the HTML into the page.

Here's an example call

```
nostojs(api => {
  api.defaultSession()
    .setResponseMode('HTML')
    .viewFrontPage()
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => {
      api.placements.injectCampaigns(response.recommendations)
    })
});
```

The `injectCampaigns` method automatically handles the injection of both the recommendations and the content. If there is no response for a specified placement, the element (into which to be injected), remains as-is.
