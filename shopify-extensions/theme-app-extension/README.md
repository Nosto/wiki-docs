# Theme App Extension

## Overview

Shopify's Theme App Extension ([more info](https://shopify.dev/docs/apps/online-store/theme-app-extensions)) was launched to improve handling of 3rd-party liquid templates, and  scripts. It simplifies adding dynamic elements or sections to the store pages isolating 3rd party content from the actual store content. This brings few changes to the way Nosto handles page tagging and script registration. This documentation outlines these changes and the process of migrating from the current approach to using the Theme App Extension.

{% hint style="info" %}
For existing merchants, It's not mandatory to migrate to Theme App Extension right away and it's possible to continue using the current approach until further notice.&#x20;

For merchants signing with Nosto post Feb 2024, Theme App Extension is enabled by default.&#x20;
{% endhint %}

## Approach

Migrating to the new Theme App Extension setup involves few changes. The following table lists these steps and it's scope (new/existing merchants)

<table><thead><tr><th width="507">Steps</th><th>New/Existing merchant</th></tr></thead><tbody><tr><td>Enabling Theme App Extension from Nosto Admin </td><td>Only Existing Merchants</td></tr><tr><td>Nosto placements to be added manually</td><td>All merchants</td></tr><tr><td>Enabling Multi-currency, Scripts and Tagging from Theme Editor</td><td>All merchants</td></tr></tbody></table>

## Enabling Theme App Extension

1. An option in Nosto Admin > Shopify dashboard can be toggled "on" to start the migration process.&#x20;

{% hint style="info" %}
In case you signed up with Nosto after Feb 2024, you are already using Theme App Extension and this option should be enabled by default.&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Migrating to Theme App Extension</p></figcaption></figure>

2. Give it atleast a minute for the process to add necessary configurations to the app for enabling the extension.
3. In case if everything went fine, please navigate to the Shopify Theme Editor. Click on the App Embed click in the left navigation, and enable both "Nosto Multi-Currency" and "Nosto Script & Tagging"

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

4. Next step is to add Nosto placements to each relevant pages (Home, Product, Collection, Cart, Search and 404).&#x20;
   1. Navigate to each page, in Theme Editor.&#x20;
   2. Click "Add section" or "Add block" (_Note: Nosto placements can only be added to the blocks that supports dynamic elements_).
   3. More info on [next page](placements-setup.md)

## Fallback

Nosto also provides a temporary fallback mechanism in case things go wrong with Theme App Extension migration. This can be done by toggling "off" the Theme App Extension option from Nosto Admin > Shopify Dashboard. Doing so reverts the setup back to the current approach.
