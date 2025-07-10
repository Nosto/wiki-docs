# Parameterless attribution

By default Nosto tracks campaign attribution without additional url parameters. The tracking happens by registering click listeners to the campaign elements that detect product url clicks and associate them with the attribution metadata of the rendered campaign. The pair of url and campaign attribution is stored in the local storage of the Browser. 

In most cases this will work out of the box, but in certain scenarious adjustments need to be made.

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

## Reliance on the legacy nosto parameters

Parameterless attribution became the default attribution mechanism on May 26th 2025. If your setup relies on the legacy nosto parameters being present you can enable the legacy behaviour in your main account settings page.