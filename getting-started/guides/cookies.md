---
description: This guide explains the Nosto usage of cookies
---

# Cookies

{% hint style="info" %}
**Disclaimer:** It is the merchant’s responsibility to ensure that all cookie-related features comply with the data-protection laws, regulations, and policies that apply in their specific country and region.
{% endhint %}

To ensure that the **Nosto Plugin** and the **Nosto Debug Toolbar** operate correctly, the following cookies must be accepted:

| Cookie                        | Purpose                                                                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2c.cid`                      | Visitor-specific identifier used by Nosto for analytics and personalisation. Set only when tracking is allowed (i.e. not in `doNotTrack` mode).     |
| `nosto-integration-allowed`   | Consent to load Nosto (essential). Replaces the former `nosto-integration-track-allow`, which is still honoured for shoppers who already consented. |
| `nosto-search-session-params` | Stores search-session parameters for more precise recommendations.                                                                                  |
| `nostoCookieFilter`           | Includes all Search/Category merchandising filters and values                                                                                       |
| `nostoCookieFilterMapping`    | Includes mapped Search/Category merchandising filters                                                                                               |
| `nosto_preview`               | Used to preview Search/Category merchandising results without enabling it globally on a live store                                                  |
| `nosto-track`                 | Marketing consent: allows Nosto to send customer data and track the visit (no `doNotTrack`).                                                        |

***

### 1. Cookie-Consent Banner

When a customer visits the store for the first time after the Nosto Plugin has been installed, Shopware displays its cookie-consent banner. The plugin adds two consent options:<br>

* A **Nosto** group - consent to load Nosto on the storefront.
* A **Nosto Marketing** entry under the **Marketing** group - consent to send customer data and enable full tracking.

> **Why it matters**\
> Nosto loads once the essential consent cookie (`nosto-integration-allowed`) is present. If the shopper accepts Nosto but **not** marketing, Nosto still loads but runs with **`doNotTrack`** enabled - no session identifier (`2c.cid`) is set and no customer data is sent. Accepting **Nosto Marketing** enables full tracking.

<figure><img src="../../.gitbook/assets/cookie banner.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

After accepting Shopware default cookie consent, cookie `nosto-search-session-params` is created which then allows Nosto script to load as well as other Nosto functionality.&#x20;

***

### 2. Send Customer Data to Nosto

Found under **Settings → Plugins → Nosto → Features flags**, this setting controls whether customer data (email, name) is sent to Nosto:<br>

* **Rely on nosto\_track cookie** _(default)_ — customer data is sent only when the shopper has accepted the marketing (`nosto-track`) cookie; otherwise Nosto runs with `doNotTrack`.
* **Always send** — customer data is always sent.
* **Never send** — customer data is never sent, and `doNotTrack` is enabled.

> **Info**\
> Backend synchronisation jobs (new orders, newsletter subscriptions) run server-side with no cookie available, so they send customer data only when this is set to **Always send**.&#x20;

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

***

### 3. Nosto Debug Toolbar

![](<../../.gitbook/assets/nosto debug toolbar.png>)<br>

The toolbar allows developers to inspect page-level Nosto events, placements, and requests while browsing the storefront.

**If you find the page load speeds a little slow, you can enable Nosto script initialization on the page interaction, that means that Nosto will only run after the user interacts with the page (scroll, click etc)**

1. Navigate to **Settings → Plugins → Nosto → All Sales Channels**.
2. In **General Settings**, enable **Initialize Nosto Script After First Page Iteration**.

<figure><img src="../../.gitbook/assets/nosto debug toolbar sw.png" alt=""><figcaption></figcaption></figure>

***

### 4. Treat Nosto as an Essential Cookie

The **Treat Nosto as an essential cookie** setting (**Settings → Plugins → Nosto → Features flags**) decides whether Nosto may load without explicit consent. **It is enabled by default.**

* **Enabled (default)** - Nosto is registered as a technically required cookie and loads for every visitor, running in `doNotTrack` mode until marketing consent is given. This matches the behaviour of earlier plugin versions.
* **Disabled** - Nosto loads only after the shopper accepts the **Nosto** cookie group in the banner. If they decline it, Nosto is never loaded and sends no requests.

{% hint style="warning" %}
This may not work with third-party cookie-consent managers - in that case, follow their documentation to allow the Nosto cookies (see section 5)
{% endhint %}

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

#### Backend steps

1. Go to **Settings → Basic Information → Security and Privacy**.
2. Disable **Use default cookie notification**.
3. (Only if it was turned off) In **Settings → Plugins → Nosto**, make sure\
   "Treat Nosto as an essential cookie" is enabled - it is enabled by default.

> **Optionally hide Shopware's banner entirely:** if explicit consent is not legally required in your region, go to **Settings → Basic Information → Security and Privacy** and disable **Use default cookie notification**. Combined with the setting enabled above, Nosto cookies are set automatically on first page load and no banner is shown.

***

### 5. Third-Party Cookie consent Manager (not Shopware default)

Third-party cookie-consent managers usually override Shopware's default banner. In that case, follow the specific manager's documentation and settings to allow the Nosto cookies.

This also means that if **Treat Nosto as an essential cookie** is enabled, it can still be blocked by a third-party manager unless configured within its settings.

Cookies that may need to be added explicitly:

* `2c.cid`
* `nosto-integration-allowed` _(load Nosto)_
* `nosto-track` _(marketing / send customer data)_
* `nosto-search-session-params`
* `nostoCookieFilter`
* `nostoCookieFilterMapping`
* `nosto_preview`

### 6. Summary Checklist

* Banner shows on first visit (unless the banner is hidden), offering the **Nosto** group and **Nosto Marketing**.
* `nosto-integration-allowed` is present after accepting Nosto; `nosto-track` after accepting marketing; `2c.cid` once tracking is allowed.
* **Send Customer Data To Nosto** is set to the desired mode (rely on cookie / always / never).
* (Optional) **Initialize Nosto Script After First Page Iteration** is enabled per sales channel.
* (Optional) **Treat Nosto as an essential cookie** is left enabled for automatic loading, or disabled for strict opt-in.

***

#### Need help?

If the toolbar does not appear or tracking seems incomplete, verify that:

If the toolbar does not appear or tracking seems incomplete, verify that:

* The required cookies are present in the browser's storage.
* No third-party script blockers are preventing Nosto from loading.

For additional assistance, contact **support@nosto.com** or consult the Nosto developer documentation.

