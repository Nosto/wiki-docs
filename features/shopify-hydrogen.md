---
description: Using Nosto's React Component Library built specifically for Shopify Hydrogen
---

# Shopify Hydrogen

[Hydrogen](https://hydrogen.shopify.dev) is a powerful React-based framework designed to create custom storefronts on Shopify. We have created a React component library as well as a demo storefront showcasing our solution for implementing Nosto in Hydrogen environments.

### Shopify Hydrogen React Component Library

Our [component library](https://github.com/Nosto/shopify-hydrogen), specifically tailored to Shopify Hydrogen, utilizes [nosto-react](https://github.com/Nosto/nosto-react) under the hood, while adding Hydrogen-specific hooks and logic to simplify Nosto integration.&#x20;

We now also support Hydrogen version 2 which is [built on Remix](https://hydrogen.shopify.dev/updates). Our [main](https://github.com/Nosto/shopify-hydrogen/tree/main) branch is dedicated to version 2, while the legacy version 1 is present on the [hydrogen-v1](https://github.com/Nosto/shopify-hydrogen/tree/hydrogen-v1) branch.

Developers can easily install our library components via [npm](https://www.npmjs.com/package/@nosto/shopify-hydrogen) into their Hydrogen-based projects to access Nosto's extensive feature set, including recommendations, onsite content personalization, dynamic bundles, pop-ups, and more. Please see below for more details.

There are two tags maintained on NPM. The `latest` version will follow`2.x.x`, while the `legacy` has version `1.x.x` to separate Hydrogen version 1 and version 2.

### Demo Store

Our [live demo store](https://shopify-hydrogen-demo.nosto.com/) is deployed via Oxygen to showcase our integration in action. You can also view the entire codebase for our store [on GitHub](https://github.com/Nosto/shopify-hydrogen-demo). This store is built on Remix using the latest version of the library.

### GitHub Repository

For further information about our Shopify Hydrogen-specific extension of nosto-react, please visit our [GitHub repository](https://github.com/Nosto/shopify-hydrogen). Our repository contains detailed [documentation](https://nosto.github.io/nosto-react/) about how to use the library, as well as other information about how it works.&#x20;

### Supported features

* Search
* Category Merchandising
* Recommendations, including client-side rendering
* Content Personalisation
* Dynamic Bundles
* Personalized Emails
* Pop-Ups&#x20;
* A/B Testing
* Segmentation & Insights&#x20;
* Analytics

### Features not currently supported

* UGC widgets
* Advanced use cases of the debug toolbar
* Dynamic placements\*

\*learn more [here](https://github.com/Nosto/shopify-hydrogen/blob/main/README.md#warning-dynamic-placements-and-shopify-hydrogen) about why the concept of dynamic placements does not apply to Hydrogen environments

{% hint style="warning" %}
We are actively working on achieving 100% compatibility with all Nosto features on Hydrogen.
{% endhint %}

### Feedback

We welcome any feedback you may have on our React component library or demo storefront. Please do not hesitate to contact us with any questions or concerns.
