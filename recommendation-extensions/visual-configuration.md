---
description: >-
  The extensions support a few flexible appearance settings that let you
  customize how content is displayed on your checkout page. Below is an overview
  of the available options and how to configure the
---

# Visual Configuration

#### How to configure your Extension

1. Open the **Shopify Checkout Editor** from your admin.
2. Click on the **extension block** you added to the checkout.
3. In the right-side panel, locate the **Block settings** section.
4. Adjust the available configuration fields such as **Aspect Ratio**, **Fit**, and **Font Size**.
5. Preview your changes live in the checkout editor.
6. Save when you're happy with the result.

#### What can be configured

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**1. Aspect Ratio**

**Field:** `aspect_ratio`\
**Description:**\
Controls the width-to-height ratio of images displayed by the extension. Use this to ensure your images appear in the correct shape and scale.

**Example values:**

* `1` for square (1:1)
* `1.5` for landscape (wider)
* `0.75` for portrait (taller)

**Default:** `1`&#x20;

**2. Image Fit**

**Field:** `fit`\
**Description:**\
Defines how the image fits within its container. This setting affects how much of the image is visible and whether it gets cropped, stretched, or resized.

**Accepted values:**

* `cover` – Fills the container, cropping if necessary (best for full-width layouts)
* `contain` – Ensures the whole image is visible inside the container
* `fill` – Stretches the image to fill the container (may distort)

**Default:** `cover`

**3. Font Size**

**Field:** `font_size`\
**Description:**\
Adjusts the size of the text used in the extension. Useful for matching your store’s tone and layout style.

**Accepted values:**

* `small`
* `medium`
* `large`

**Default:** `medium`

