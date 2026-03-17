# Parameterless Attribution

By default Nosto tracks campaign attribution without additional url parameters. The tracking happens by registering click listeners to the campaign elements that detect product url clicks and associate them with the attribution metadata of the rendered campaign. The pair of url and campaign attribution is stored in the local storage of the Browser.

In most cases this will work out of the box, but in certain scenarios adjustments need to be made. For a comprehensive overview, please read our [personalization attribution guide](../../implement-psn/README.md#attribution).

## Support for non anchor clicks

In addition to handling product url navigation attribution it is also possible to annotate elements with a `data-nosto-product` attribute to track interactions with other elements that should be attributed to the campaign. The attribute value should be a valid product id.

This attribution style should only be used for element interactions that don't trigger page reloads. Examples are product detail drawers and overlays as well as add to cart dialogs.

## Product url redirects

In case the product urls used in Nosto campaigns have HTTP level redirects applied the HTML should link back to the canonical url used in Nosto campaign via `link[refl="canonical"]` elements in the head element. Nosto uses both the current location and the canonical page url as lookup keys for the attribution metadata.

## Session API based usage

When combined with Session API based requests and HTML based campaign results it is advisable to let the Nosto API handle the campaign injection by enabling campaign injection on the session level:

```js
api.defaultSession()
  .setResponseMode("HTML")
  .enableCampaignInjection()
  .viewProduct(...)
  .setPlacements(...)
  .load() 
```

Check out the API documentation for [defaultSession](https://nosto.github.io/nosto-js/interfaces/client.API.html#defaultsession)

## Reliance on the legacy nosto parameters

Parameterless attribution became the default attribution mechanism on May 26th 2025. If your setup relies on the legacy nosto parameters being present you can enable the legacy behavior in your main account settings page.

## JS API based usage: JSON Rendering Attribution

### Attribution in custom element based Nosto campaign rendeirng

Below is an example of a custom element that fetches JSON results based on the placement attribute, renders them and register parameterless attribution for product link clicks:

```js
export class NostoRenderer extends HTMLElement {
  async connectedCallback() {
    const api = await new Promise(nostojs)
    const placement = this.getAttribute("placement")
    if (placement) {
      const results = await api
        .createRecommendationRequest({ includeTagging: true })
        .setElements([placement])
        .setResponseMode("JSON_ORIGINAL")
        .load()
      if (results.recommendations[placement]) {
        const rec = results.recommendations[placement]
        const container = document.getElementById(placement)
        // TODO: Define your own method to render products
        renderProductsToContainer(container, recommendation)
        api.attributeProductClicksInCampaign(this, rec)
      }
    }
  }
}

if (!customElements.get("nosto-renderer")) {
  customElements.define("nosto-renderer", NostoRenderer)
}
```

### Rendering of campaign markup in non-managed placement elements

In case the campaign markup is rendered into a non-placement element the element will need to be registered with parameterless attribution handling via `api.attributeProductClicksInCampaign`:

```js
const placementId = "frontpage-nosto-1"

const response = await api
 .createRecommendationRequest({ includeTagging: true })
 .setResponseMode("JSON_ORIGINAL")
 .setElements([placementId])
 .load()

const recommendation = response.recommendations[placementId]
const container = document.getElementById(placementId)
if (recommendation && container) {
  // TODO: Define your own method to render products
  renderProductsToContainer(container, recommendation)
  api.attributeProductClicksInCampaign(container, recommendation)
}
```

Check out the API documentation for [attributeProductClicksInCampaign](https://nosto.github.io/nosto-js/interfaces/client.API.html#attributeproductclicksincampaign)
