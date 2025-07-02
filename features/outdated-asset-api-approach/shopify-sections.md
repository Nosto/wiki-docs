# Shopify Sections

* [Shopify sections](shopify-sections.md#shopify-sections)
  * [Introduction](shopify-sections.md#introduction)
  * [How it works](shopify-sections.md#how-it-works)
  * [Add new placement](shopify-sections.md#add-new-placements)

## Introduction

Nosto supports Shopify sections. You can add new placements through sections or drag-and-drop existing ones.

{% hint style="info" %}
Our implementation for sections is limited only to Shopify Online Store 2.0. This is based on Shopify architecture. [Read more](https://shopify.dev/themes/architecture/sections/section-schema#access-section-settings)
{% endhint %}

## Enable sections feature

To enable sections navigate to the `Shopify Settings` tab in your Nosto account, or `Settings` > `Integrations` > `Shopify` > `Manage`. You can find it easily under `Dashboard > Go to Shopify Settings`. On the settings page, navigate to the `Settings` tab click the enable button.

{% hint style="info" %}
To add sections to existing themes with Nosto please remove Nosto from the theme and install again, or refresh theme.
{% endhint %}

![image](https://user-images.githubusercontent.com/44775916/190137971-e989aeef-25fa-4b55-9e0a-c9dc666dbf16.png)

## How it works

When enabled on Nosto's end, our app will create a new section under `sections` named `nosto-placement.liquid`. The default Nosto placements will be added on the template JSON file.

![image](https://user-images.githubusercontent.com/44775916/169779382-ead881c6-06f7-42c4-82f4-19ad0ce62941.png)

You can find the placements on the theme editor under the name of `Nosto placement`. The placement id is passed as a setting parameter.

![image](https://user-images.githubusercontent.com/44775916/169779615-833226bb-36cc-4dc6-92f1-3e376ef626fa.png)

## Add new placements

You can add more placements to your theme based on the section. In case you are adding through the code editor, you need to pass the placement id under the settings object with key `placement`.

```json
    "productpage-nosto-1": {
      "type": "nosto-placement",
      "settings": {
        "placement": "productpage-nosto-1"
      }
    }
```
