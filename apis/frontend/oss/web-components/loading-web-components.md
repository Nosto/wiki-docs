# Loading Web components

## Introduction

As \`@nosto/web-components\` is an open-source package, it is available through multiple sources

1. From [npm](https://www.npmjs.com/package/@nosto/web-components) registry
2. From [jsdeliver](https://cdn.jsdelivr.net/npm/@nosto/web-components@latest/dist/main.es.bundle.js) CDN

This provides us with the following three different ways to load web-components artifact for usage in recommendation templates. &#x20;

1. From Nosto's Admin UI, Product Experience Cloud > Recommendations > Settings (recommended)
2. Using script tag module, in every recommendation templates, see [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#applying_the_module_to_your_html)

Following sections details each of these steps and it's advantages/disadvantages

{% hint style="info" %}
Fixed version usage

We recommended using a fixed version name, instead of latest, when loading the web components. As the web-component package is fairly new, it will undergo numerous changes in coming days and backward-compatibility is not guaranteed in all the new major releases.&#x20;

When loading web-components from the Nosto's admin UI is recommended and considered safer as it mandates the use of specific version for this reason.&#x20;
{% endhint %}

### Approach #1: Using Recommendation Settings

A new toggle, to control loading of web components artifact, has been introduced in Recommendation settings page. Enabling this option requires specifying the version of web-components artifact to load when recommendations are rendered in store pages.&#x20;

For all the available web components version, refer the [Available Web components versions](https://www.npmjs.com/package/@nosto/web-components?activeTab=versions) link provided in the settings page.&#x20;

{% hint style="info" %}
Recommended approach for live recommendations.

When you start from our default `<platform>-swatches` template (in case of Shopify [shopify-swatches](https://my.nosto.com/admin/shopify/templates/6814a8fa19bab54e44927d37)), it's recommended to remove the \<script> tag to load the artifact from the template before enabling the web components in the settings.&#x20;
{% endhint %}

#### Advantages

1. Controlled by Nosto and loaded automatically along with the client script. Hence the artifact need not be included in every recommendation template&#x20;
2. Updating the version is easy as it is controlled by the setting instead of having to change the version in every recommendation template

#### Disadvantages

1. Version control is global. So, when the version is updated in settings, it's applied to all the recommendation templates. Can't be set for every page.
2. Web components artifact is loaded on all pages even when the recommendations on page doesn't use web components

<figure><img src="../../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Approach #2: using Script tag (in recommendation templates)

When you don't want to enable web components globally from the recommendation settings for some reason, we can use this approach to include the web components artifact directly in individual recommendation templates.&#x20;

Web components artifact is an ES module so they can be included using the script tag as shown below

```
<script type="module" src="https://cdn.jsdelivr.net/npm/@nosto/web-components@4.0.0/dist/main.es.bundle.js" />
```

#### Advantages

1. Web components artifact version can be chosen separately for each templates individually. This will come in handy when a version doesn't work in a template and a fix for the issue is released in a newer version. Please note that web components version is page scoped (front, product, cart etc...). Only one version can be loaded per page.&#x20;
2. Web components artifact is loaded on when it's used in a recommendation template on page

#### Disadvantages

1. Script tag needs to be included in each recommendations individually
2. Maintaining the version or upgrading to a new version in each recommendations can be a painful process

