# Installing

## Requirements

Nosto integration does not support legacy Pipelines. You have to use SiteGenesis or [Storefront Reference Architecture \(SFRA\)](https://www.salesforce.com/products/commerce-cloud/resources/commerce-cloud-storefront-reference-architecture/)

## Installing the integration

To install the Nosto integration for the first time on their instance, user may perform the following steps:

1. Clone repository to local machine.
2. Import the cartridge \(int\_nosto\) into your workspace and link it to the Digital Server Connection. Note that `int_nosto_sfra` cartridge is also provided but it is not mandatory to use the cartridge as it is just representation of how the cartridge will work with SFRA.
3. To import site’s metadata, create zip file for metadata folder \(metadata.zip\). Import metadata.zip into your instance \(Meta Data and Custom Objects and Schedules\).
4. Assign the int\_nosto cartridges to site’s cartridge path. It should be configured in below sequence. The cartridge path sequence should look like this:

   **Cartridge path for controllers \(SiteGenesis\)**

   `int_nosto: app_storefront_controllers: app_storefront_core`

   **Cartridge path for SFRA** `int_nosto: app_storefront_base`

Add `int_nosto_sfra` cartridge before `int_nosto` to understand what changes are needed in the base cartridge and to understand how the cartridge works. `int_nosto_sfra` is created just to simulate how the cartridge will work and you have to make sure similar changes are copied over to respective base cartridge as mentioned in the implementation section.

