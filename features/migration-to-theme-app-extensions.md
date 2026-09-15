---
description: >-
  This page describes how you can migrate your Nosto installation from the
  outdated approach to using Theme App Extensions.
---

# Migration to Theme App Extensions

## Important dates

**Shopify is retiring Script Tags for apps. Two dates matter for your store:**

* **01.10.2026:** Shopify no longer lets apps create or update Script Tags. From this day, Nosto cannot register or repair a Script Tag for your store. An existing Script Tag keeps working as long as nobody removes it.
* **01.03.2027:** Shopify stops loading Script Tags on storefronts entirely. Any store still relying on the Nosto Script Tag will stop showing Nosto experiences and stop sending shopper signals from the storefront.

If your store still loads Nosto through a Script Tag, **complete the steps under "The minimum you need to do" before 01.10.2026**. After that date, a store that loses its Script Tag cannot get it back, and the only way forward is the Theme App Extension.

Not sure if your store is affected? See "How to check if your store is already migrated" below.

## Why this is needed

Shopify has deprecated legacy Script Tag methods for app integration as well as their Asset API, in favor of Theme App Extensions. This is not just a technical change, it is a shift toward a more robust, standardized, and merchant-friendly integration model.

Following this, Nosto launched Theme App Extensions support in spring 2024 as the new integration standard. Since then, this approach proved highly valuable, introducing many upsides. It does not stop you from still using hard-coded placements you added to your theme, and only simplifies the creation of placements and control over the Nosto script and Nosto tagging.

Shopify has now set the dates above, so the migration is no longer optional.

### Key Benefits&#x20;

* **Modern standard**: Theme App Extensions are the new default for Theme adjustments in Shopify.
* **Unified Theme-Management**: Nosto automatically works across all of your Themes, reducing the time to bring your Nosto Experience to new Theme Versions.
* **Full Control**: Nosto Placements are fully embedded, allowing control and previewing in Shopify's Theme Editor.
* **Future-proof**: Script Tags are no longer considered a supported or recommended method. All Nosto features are built with Theme App Extensions in mind for a while now.

☑️ Following this, we're now migrating also existing merchants to Theme App Extensions completely.

{% hint style="info" %}
While we recommend using the Theme App Extension including Theme App Blocks and the App Embed Script, you can of course continue using hard-coded Placements that you add to your files, call Nosto APIs for results and more.&#x20;
{% endhint %}

## About the Migration

This migration replaces the way Nosto's script and tagging get into your theme. For older installations, Shopify injects a Nosto Script Tag and Nosto has added tagging files to your theme. After the migration, both come from the Nosto Theme App Extension instead, which you control in your Theme Editor under App Embeds.

It is **not** related to Shopify's other new extension features, and it does **not** change your Nosto campaigns, placements, Search or Category Merchandising setup. Custom sections or hard-coded placements built by your developers continue to work.

Every store that was installed before spring 2024 and has not migrated yet is affected, whether or not you use Recommendations. The Script Tag loads everything Nosto does on your storefront.

### Script Migration - the minimum you need to do

If you cannot complete the full migration before 01.10.2026, this is the smallest change that protects your store. Do it on your **published** theme:

1. Enable the **Nosto Script** App Embed in your Theme Editor (see "Before you start" below for where to find it). This replaces what the Script Tag does today. Your existing Nosto tagging in the theme can stay as it is for now.
2. In your Nosto Dashboard, click on "Go to Shopify Settings". Confirm that **Nosto App Embed** shows **Active**. This tells you the embed is live on your published theme and is loading Nosto. If this is not the case yet, click on "Open Theme Editor" to enable the App Embed Script, then come back and refresh.
3. Once the embed shows Active, click **Remove Script Tag** in the same panel. The **Script Tag** row then flips from **Still registered** to **Removed**
4. Both rows should now show green, and your store is safe for both deadlines on the minimum path.

{% hint style="info" %}
Removing the Script Tag before activating the **Nosto App Embed** takes prevents Nosto from loading in your store, and after 01.10.2026 we cannot re-register the Script Tag for you.&#x20;

If the embed does not show Active in your Nosto admin, do not remove the script tag yet - make sure the **Nosto Script** App Embed is enabled on your **published** theme (see "Before you start"), then check the panel again.&#x20;

The only exception is, in case you are manually loading Nosto from a `<script>`  element in your theme.
{% endhint %}

Please keep in mind, that this is the minimum. Shopify has also deprecated the Asset API that the old tagging and placement files rely on. There is no date for final end-of-life yet, but we can expect this will change on some point. The only setup that is safe for both changes is the full migration below, with **Nosto Script** and **Nosto Tagging** both enabled and the old theme files removed. Plan it for a quieter moment after the deadline if you cannot do it now.

### Full Migration - Recommended path

Your store is only fully migrated when, in addition to the Script migration, the **Nosto Tagging** App Embed toggle is enabled and the old Nosto theme files are removed.

{% hint style="warning" %}
We highly suggest to prepare your Migration on a non-live Theme (e.g. on a copy of your current Theme)
{% endhint %}

Before proceeding, please verify the above steps of **Script migration - the minimum you need to do** are done. Then:

1. **Navigate to "App Embeds" in your Theme**
   * In Shopify, open your Theme Editor
   * Navigate to "App Embeds"
2. **Nosto Tagging (Mandatory):**
   * This embeds Nosto Tagging (e.g. for page types) in your theme. This is required to share needed context with Nosto, allowing you to contextually load Nosto placements & more.
   * **Multi-currency Settings (Optional):**
     * This functionality will embed Multi-Currency tagging in your theme, allowing Nosto to understand your currencies as well as fetching conversion rates. Only needed when operating in Multi-Currency setups.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Tagging Migration Options

There are two possible paths to complete your tagging migration. You can choose between a Quick Version or a Clean Version (our Recommendation). Your choice depends on whether you're aiming for minimal disruption or a long-term clean setup. Both options will be outlined in the following section.

#### Quick Version

This version is for you, if you want to migrate with the least possible effort.&#x20;

Using this approach, you will **retain all** out-of-the box & hard-coded placements, that initially have been added for you when you first installed Nosto to your theme, and only remove the old tagging and script version. &#x20;

<details>

<summary><strong>Quick Version - Details</strong></summary>

{% hint style="warning" %}
Removing Nosto will also delete the files `nosto-tagging.liquid` and `nosto-element.liquid`, which were added to your theme during installation. It will also remove their references in theme.liquid.

**Before proceeding, make sure that**:\
• Your theme is not dependent on these files (search within your theme to confirm).\
• You have not modified these files for custom placements.
{% endhint %}

**Upsides:**&#x20;

* Migration only takes a couple of minutes
* You will not need to replace existing Placements in any way
* Future-Placements can be added using Theme Blocks

**Downsides:**&#x20;

* Existing Placements that have been hard-coded (Static Placements) into your theme will stay there, making Placements spread in different places&#x20;
* Those older Placements will live outside of Nosto's control, and can't be removed or controlled by Nosto anymore
* You will need to access your Theme files in order to move these Placements, or to remove them

#### How to do it

1.  Go to your _Nosto Dashboard_ -> click "_Go to Shopify_ _Settings"_  (upper right corner)

    <figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>


2.  In your Theme Management Section, click on "_Partially remove Nosto_", which will remove the old script and old tagging from your Theme code, but **retains your Placements fully**<br>

    <figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>



</details>

#### Clean Version (Recommended)

This version is for you, if you want to migrate in a structured way, that allows you full control and easy usability going forward.&#x20;

Using this approach, you will **replace** all hard-coded/static placements manually, and instead fully migrate to App-Sections, embedding Nosto Placements in Theme Blocks. This ensures a consistent integration, removing unclarity on where a placement actually sits going forward.&#x20;

<details>

<summary><strong>Clean Version - Details</strong></summary>

{% hint style="warning" %}
Removing Nosto will also **delete all Nosto files** that were added to your theme during installation, as well as their references in theme.liquid.

**This includes files such as**:\
• `nosto-placement.liquid`\
• `nosto-element.liquid`\
• `nosto-tagging.liquid`

It will also remove **all references to these files** within your theme’s pages (for example: `index`, `product`, etc.).

Before proceeding, make sure that:\
\
• Your theme does not depend on these files (search your theme for references to them).\
• You have not modified these files for custom placements.
{% endhint %}

**Upsides:**&#x20;

* Going forward, you will have all of your Nosto Placements fully embedded in Theme Blocks, unlocking all the flexibility of Shopify's Theme Editor.&#x20;
  * Dynamic Placements can still be created to target CSS selectors of course.
* You will avoid unclarity about where a Nosto campaign can be controlled, avoiding a mixture of hard-coded/static Placements & Theme Blocks
* Placements embedded in Theme Blocks, will allow you to preview Nosto campaigns right in your Theme Editor
* This will ensure you're getting the most out of Nosto, minimising Nosto's site-speed impact automatically
* You will run your setup fully in latest technology, ensuring you have all upcoming improvements and capabilities available as well
* When you would uninstall Nosto, these Placements would be fully removed as well

**Downsides:**&#x20;

* Migration is more manual, requiring you to replace your existing Placements one by one\
  Depending on your amount of hard-coded/static placements, this might take a while
* Nosto can't detect the exact spots where your old Placements sit, so you will need to manually capture them
* There will be a short down-time for Placement-related campaigns (Recommendations, Bundles, OCP)

#### How to do it &#x20;

1. Capture your current Placements, ensuring you know where on the site they appear. (You can find a list of all out-of-the-box placements here\[LINK])
   * You could e.g. take Screenshots, make a list or similar. You will need the Placement ID later on, so make sure to capture this as well
   * **Important:** Nosto will not be able to remove manually created Placements that you hard-coded into your theme files - if you have other placements than mentioned here\[LINK], you will need to manually remove them)
2.  When you're done, go to your _Nosto Dashboard_ -> click "_Go to Shopify_ _Settings"_  (upper right corner)

    <figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>


3.  In your Theme Management Section, click on "_**Fully remove Nosto**_", which will remove the old script, old tagging and your out-of-the-box placements fully.

    <figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>


4. Now go back to your Theme Editor in Shopify, and use your captured list to add your campaigns back where your old Placements used to be.

* Find the place a campaign should appear
* Add a Section & choose _Apps -> Nosto_
* Add your placement ID&#x20;

<mark style="color:yellow;">**💡**</mark>**&#x20;**<mark style="color:$primary;">**Note:**</mark> **If you want to minimize downtimes, you can also replace Placements one-by one**

* Open two tabs in your browser&#x20;
  * One for your Theme Editor
  * One for your Theme Files
* Identify your Placement in the Files, as well as the same place in the Theme Editor
* Remove the hard-coded Placement
* Immidiately replace it with a Theme Block

Using this approach, you will only have seconds without having a campaign in live.

</details>

In case you have any questions or concerns, please let us know.&#x20;
