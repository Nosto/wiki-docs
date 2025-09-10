---
description: >-
  This page describes how you can migrate your Nosto installation from the
  outdated approach to using Theme App Extensions.
---

# Migration to Theme App Extensions

## Why this is needed

Shopify has deprecated legacy Script Tag methods for app integration as well as their Asset API, in favor of Theme App Extensions. This is not just a technical change it’s a shift toward a more robust, standardized, and merchant-friendly integration model.

Following this, Nosto launched Theme App Extensions support in spring 2024 as the new Integration-Standart. Since then, this approach proved highly valuable, introducing many upsides. This approach doesn't stop you from still using hard-coded and custom placements, and only simplifies the creation of placements and control over Nosto script/Nosto tagging.

### Key Benefits&#x20;

* **Performance**: Faster script loading and natively embedded Placements reduce Nosto's affect on site speed drastically.
* **Modern standard**: Theme App Extensions are the new default for Theme adjustments in Shopify.
* **Unified Theme-Management**: Nosto automatically works across all of your Themes, reducing the time to bring your Nosto Experience to new Theme Versions.&#x20;
* **Full Control**: Nosto Placements are fully embedded, allowing control and previewing in Shopify's Theme Editor.
* **Future-proof**: Script Tags are no longer considered a supported or recommended method. All new features will be built with Theme App Extensions in mind.

{% hint style="success" %}
Following this, we're now migrating also existing merchants to Theme App Extensions completely.&#x20;
{% endhint %}

## Before you start

Before proceeding, please verify the following conditions to avoid issues or downtimes when in the process. App Embeds are per Theme which means if you enable Nosto Script on one theme, it will not be automatically enabled on other themes. \
\
Enabling Nosto Script on a Live Theme can be done out of the box which would provide faster script loading, even without migration of placements!\


1. **Navigate to "App Embeds" in your Theme**
   * In Shopify, open your Theme Editor
   * Navigate to "App Embeds"
2. **Enable Nosto-Settings in "App Embeds"**
   * **Nosto Script (Mandatory):**
     * This functionality embeds the Nosto script into your theme, enabling all Nosto functionality. This is mandatory - Nosto cannot load otherwise.
   * **Nosto Tagging (Mandatory):**
     * This embeds Nosto Tagging (e.g. for page types) in your theme. This is required to share needed context with Nosto, allowing you to contexually load Nosto placements & more.
   * **Multi-currency Settings (Optional):**
     * This functionality will embed Multi-Currency tagging in your theme, allowing Nosto to understand your currencies as well as fetching conversion rates. Only needed when opperating in Multi-Currency setups.&#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you can't find this App Embed in your Theme, or in case either of these Toggles is missing, please reach out to Nosto Support!
{% endhint %}

## Starting the Migration

There are two main paths to complete your migration. You can choose between a **Quick Version** or a **Clean Version** (Recommended). Your choice depends on whether you’re aiming for minimal disruption or a long-term clean setup. Both options will be outlined in the following section.\
\
Make sure that any migration attempts are done on **unpublished theme** (make a copy of a Live theme if needed)

### Quick Version

This version is for you, if you want to migrate with the least possible effort.&#x20;

Using this approach, you will **retain all** out-of-the box & hard-coded placements, that initially have been added for you when you first installed Nosto to your theme, and only remove the old Tagging- and Script-Version.  \
\
Be mindful that this will **remove** "nosto-tagging.liquid" and "nosto-element.liquid"  files that were created by Nosto when it was installed to your theme as well as these files reference in "theme.liquid". Please make sure that your theme and other files are not calling these files, and that these files weren't modified by you for custom placements before proceeding.&#x20;

<details>

<summary><strong>Quick Version - Details</strong></summary>

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


2.  In your Theme Management Section, click on "_Partially remove Nosto_", which will remove the old script and old tagging from your Theme code, but **retains your Placements fully**\


    <figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>



</details>

### Clean Version (Recommended)

This version is for you, if you want to migrate in a structured way, that allows you full control and easy usability going forward.&#x20;

Using this approach, you will **replace** all hard-coded/static placements manually, and instead fully migrate to App-Sections, embedding Nosto Placements in Theme Blocks. This ensures a consistent integration, removing unclarity on where a placement actually sits going forward. \
\
Be mindful that this will **remove** all nosto files that were created by Nosto when it was installed to your theme as well as these files reference in "theme.liquid". This include "nosto-placement.liquid", "nosto-element.liquid", "nosto-tagging.liquid" etc as well as all references to these files within your themes pages (such as index, product etc). Please make sure that your theme and other files are not calling these files, and that these files weren't modified by you for custom placements before proceeding.&#x20;

<details>

<summary><strong>Clean Version - Details</strong></summary>

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
