---
description: This guide explains the Nosto usage of cookies
---

# Cookies

{% hint style="info" %}
**Disclaimer:** It is the merchant’s responsibility to ensure that all cookie-related features comply with the data-protection laws, regulations, and policies that apply in their specific country and region.
{% endhint %}

To ensure that the **Nosto Plugin** and the **Nosto Debug Toolbar** operate correctly, the following cookies must be accepted:

| Cookie                          | Purpose                                                                      |
| ------------------------------- | ---------------------------------------------------------------------------- |
| `2c.cid`                        | Visitor-specific identifier used by Nosto for analytics and personalisation. |
| `nosto-integration-track-allow` | Indicates that Nosto tracking is permitted.                                  |
| `nosto-search-session-params`   | Stores search-session parameters for more precise recommendations.           |

***

### 1. Cookie-Consent Banner

When a customer visits the store for the first time after the Nosto Plugin has been installed, Shopware displays its default cookie-consent banner.

> **Why it matters**\
> These cookies load essential script files—including the Nosto Debug Toolbar—so declining them can prevent the plugin from functioning.

<figure><img src="../../.gitbook/assets/cookie banner.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/cookie preferences.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/nosto cookies.png" alt=""><figcaption></figcaption></figure>



***

### 2. Nosto Debug Toolbar

![](<../../.gitbook/assets/nosto debug toolbar.png>)\


The toolbar allows developers to inspect page-level Nosto events, placements, and requests while browsing the storefront.

**If you find the page load speeds a little slow, you can enable Nosto script initializiation on the page interaction, that means that Nosto will only run after the user interacts with the page (scroll, click etc)**

#### Plugin prerequisites

1. Navigate to **Settings → Plugins → Nosto → All Sales Channels**.
2. In **General Settings**, enable **Initialize Nosto Script After First Page Iteration**.

<figure><img src="../../.gitbook/assets/nosto debug toolbar sw.png" alt=""><figcaption></figcaption></figure>

***

### 3. Automatic Cookie Acceptance (Optional)

If explicit cookie consent is **not** legally required in your region, you can configure Shopware to accept Nosto cookies automatically.

#### Backend steps

1. Go to **Settings → Basic Information → Security and Privacy**.
2. Disable **Use default cookie notification**.\

3. Return to **Settings → Plugins → Nosto** and enable **Ignore cookie consent**.

<figure><img src="../../.gitbook/assets/ignore cookie consent configuration.png" alt=""><figcaption></figcaption></figure>

> **Result**\
> Customers will no longer see the cookie-consent banner, and Nosto cookies will be set automatically on the first page load.

***

### 4. Summary Checklist

Banner shows on first visit (unless automatic acceptance is enabled).

`2c.cid`, `nosto-integration-track-allow`, and `nosto-search-session-params` are present after consent.

(Optional) **Initialize Nosto Script After First Page Iteration** is enabled for every sales channel

(Optional) **Ignore cookie consent** is enabled when automatic acceptance is desired.

***

#### Need help?

If the toolbar does not appear or tracking seems incomplete, verify that:

* All three cookies are present in the browser’s storage.
* No third-party script blockers are preventing Nosto from loading.

For additional assistance, contact **support@nosto.com** or consult the Nosto developer documentation.

