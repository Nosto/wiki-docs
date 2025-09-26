# Loading Web Components

## Introduction

Since [Nosto Web Components](https://github.com/Nosto/web-components) is an open-source package, it is available through different channels:

1. From the [npm](https://www.npmjs.com/package/@nosto/web-components) registry
2. From the [jsDelivr](https://cdn.jsdelivr.net/npm/@nosto/web-components@latest/dist/main.es.bundle.js) CDN

This gives us two ways to load the web components artifact for use in recommendation templates:

1. Using the client script, configured through Nosto's Admin UI under Product Experience Cloud > Recommendations > Settings (recommended)
2. Using module scripts in every recommendation template (see [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#applying_the_module_to_your_html))

The following sections detail each approach along with its advantages and disadvantages.

{% hint style="info" %}
Fixed Version Usage

We recommend using a fixed version instead of "latest" when loading the web components. Since the web components package is relatively new, it is expected to undergo frequent changes, and backward compatibility is not guaranteed across all new major releases.

Loading web components via Nosto's Admin UI is considered safer, as it enforces the use of a specific version.
{% endhint %}

### Approach #1: Using the Client Script

A new toggle has been introduced on the Recommendation Settings page to control the loading of the web components artifact. Enabling this option requires specifying the version of the artifact to load when recommendations are rendered on store pages.

For a list of all available versions, refer to the [Available Web Components Versions](https://www.npmjs.com/package/@nosto/web-components?activeTab=versions) link provided on the settings page.

{% hint style="info" %}
Recommended Approach for Live Recommendations

If you start with our default `<platform>-swatches` template, it is recommended to remove the `<script>` tag that loads the artifact from the template before enabling web components in the settings.
{% endhint %}

#### Advantages

1. Controlled directly by Nosto and loaded automatically via the client script, eliminating the need to include the artifact in every recommendation template.
2. Updating the version is straightforward since the version is controlled via settings rather than having to change it in every recommendation template.

#### Disadvantages

1. Version control is global; updating the version in settings applies it to all recommendation templates, so it cannot be set individually for each page.
2. The web components artifact is loaded on all pages, even when a particular page does not utilize web components.

<figure><img src="../../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Approach #2: Using a Script Tag in Recommendation Templates

If you prefer not to enable web components globally via the recommendation settings, you can include the web components artifact directly within individual recommendation templates.

Since the web components artifact is an ES module, include it using a script tag as shown below:

```
<script type="module" src="https://cdn.jsdelivr.net/npm/@nosto/web-components@8.24.0/dist/main.es.bundle.js"></script>
```

#### Advantages

1. The version of the web components artifact can be chosen individually for each template. This is beneficial when a particular version does not work in a template, and a fix for the issue is released in a newer version. Please note that the web components version is page-scoped (for example, front, product, cart, etc.), meaning only one version can be loaded per page.
2. The web components artifact is loaded only when it is used in a recommendation template.

#### Disadvantages

1. The script tag must be included in each recommendation template individually.
2. Maintaining and upgrading the version in every recommendation template can be a tedious process.
