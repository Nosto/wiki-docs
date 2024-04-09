# Product Variant handling

### Overview

This plugin offers detailed configuration options for sending product data to Nosto, allowing you to control how products are displayed in your storefront. You have several options for how to display products and expand property values in product listings.

#### Terminology

* **Main Product**: Refers to the parent product, the configured main variant, or the cheapest variant. Products are exported based on Shopware's display logic in search or category result pages, subject to certain conditions.
* **Variants**: Sibling products of the main product, subject to the same export criteria as the main product.

### Export Configuration

#### Criteria for Product Export

Products must meet the following conditions to be eligible for export to Nosto:

* Products can be active or inactive.
* Product is assigned to a sales channel linked with the configured Account ID.
* All exported products must include an image, as required by Nosto.

#### Variant Export Options

Variants can be exported in two specific ways without requiring global configuration:

1. **As One Main Product**: Combines all variant information into a single product listing.
   * Main-/Parent product.
   * Cheapest variant.
2. **As Separate Products**:
   * **With a Selected Value (e.g., Colour)**: Separate products for each colour, including information on other variants with the same colour.
   * **With All Values Selected**: Separate products for each variant.

### Display Options for Single Products

You can configure how individual products or variants are displayed in your storefront:

* **Main Product**: Sends the main or parent product to Nosto with its variants (child products) associated.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* **Cheapest Variant**: Automatically displays the cheapest variant available to customers.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* **Variant Product**: Sends the selected variant (child) to Nosto.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

### Expand Property Values in Product Listings

Control how products with different properties (e.g., color, size) are listed:

* **With a Selected Value (e.g., Colour)**: Lists separate products for each color. Each listing includes information on other variants with the same color.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

* **With All Values Selected (e.g., Colour, Size)**: Lists separate products for each variant.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12).png" alt="" width="563"><figcaption></figcaption></figure>

* **Without Selected Values**: This is not recommended and can lead to unexpected behaviour.



<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

###

