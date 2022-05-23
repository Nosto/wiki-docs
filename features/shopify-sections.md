# Shopify Sections

* [Shopify sections](shopify-sections.md#shopify-sections)
    * [Introduction](shopify-sections.md#introduction)
    * [How it works](shopify-sections.md#how-it-works)
    * [Add new placement](shopify-sections.md#add-new-placements)



## Introduction
Nosto supports Shopify sections. You can add new placements through sections
or drag-and-drop existing ones.


```
Warning!
Our implementation for sections is limited only to Shopify Online 
Store 2.0. This is based on Shopify architecture.
```
[Read more](https://shopify.dev/themes/architecture/sections/section-schema#access-section-settings)

## How it works

When enabled on Nosto's end, our app will create a new section under `sections` named `nosto-placement.liquid`.
The default Nosto placements will be added on the template JSON file.

<img width="1129" alt="image" src="https://user-images.githubusercontent.com/44775916/169779382-ead881c6-06f7-42c4-82f4-19ad0ce62941.png">


You can find the placements on the theme editor under the name of `Nosto placement`. The placement id is passed as a setting parameter.

<img width="1792" alt="image" src="https://user-images.githubusercontent.com/44775916/169779615-833226bb-36cc-4dc6-92f1-3e376ef626fa.png">


## Add new placements

You can add more placements to your theme based on the section. 
In case you are adding though the code editor, you need to pass the placement id under the settings object with key `placement`.

```json
    "productpage-nosto-1": {
      "type": "nosto-placement",
      "settings": {
        "placement": "productpage-nosto-1"
      }
    }
```