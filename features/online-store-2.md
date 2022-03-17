# Online Store 2.0

- [Online Store 2.0](#online-store-20)
  - [Summary](#summary)
  - [Nosto's implementation](#nostos-implementation)
    - [Existing Shopify themes](#existing-shopify-themes)
    - [Online 2.0 themes](#online-20-themes)
      - [Existing JSON templates](#existing-json-templates)
      - [New liquid templates](#new-liquid-templates)
    - [Hybrid themes](#hybrid-themes)
  - [Migrating customers](#migrating-customers)
  - [Limitations](#limitations)

## Summary

Shopify has recently released a version 2.0 of their online themes with numerous changes and enhancements, notably the migration from `.liquid` template files to `.json` template files. This impacted the process of installing Nosto in any of the version 2.0 themes as Nosto mostly expects and modifes `.liquid` files. Nosto has recently released support for version 2.0 themes and this documentation will discuss the details in-depth.
For more information Shopify side of changes, visit corresponding links below,
1. [Online 2.0 themes](https://www.shopify.com/partners/blog/shopify-online-store)
2. [JSON Templates](https://shopify.dev/themes/architecture/templates/json-templates)
3. [Migration Guide](https://shopify.dev/themes/os20)

## Nosto's implementation

After the recent update, Nosto supports for the following,
1. Existing Shopify themes (let's call it version 1.0 themes)
2. Online store 2.0 themes (let's call it version 2.0 themes)
3. Hybrid themes (will discuss more shortly)

### Existing Shopify themes

Support and process for version 1.0 of shopify themes will remain unchanged

### Online 2.0 themes

In order to support the new `JSON` templates and inject Nosto snippet, following changes will be made to the installed theme (For e.g, the [Dawn](https://themes.shopify.com/themes/dawn/styles/default) theme). 

#### Existing JSON templates

> Following table lists existing JSON templates that will be updated while installing Nosto

1. templates/index.json
2. templates/404.json
3. templates/collection.json
4. templates/product.json
5. templates/search.json
6. templates/cart.json

#### New liquid templates

> Each of the section values/properties introduced in the section above needs the corresponding liquid present under sections group of the theme. Following sections will list all the new liquid template files that will be introduced along with it's content

1. sections/nosto-404.liquid
2. sections/nosto-cart.liquid
3. sections/nosto-collection-prepend.liquid
4. sections/nosto-collection-append.liquid
5. sections/nosto-index.liquid
6. sections/nosto-product.liquid
7. sections/nosto-search-prepend.liquid
8. sections/nosto-search-append.liquid

### Hybrid themes

As per their migration guide, Shopify allows the option of migrating their themes one file at a time, meaning a customer can migrate `templates/404.liquid` to `templates/404.json` today and leave the rest of their theme setup as it is and continue using the liquid template files. In that case, a customer will be using scheme structure of both `version 1.0` and `version 2.0` themes. Nosto has implemented support for this scenario under the recent release. It will automatically identify those files that are still using `version 1.0` liquid templates and injects content, into the liquid template, using the existing logic. Also identifies those that belongs to `version 2.0` json templates and injects content, into those json templates, using the approach discussed in above sections.


## Migrating customers

Assume a customer is on `version 1.0` and has Nosto installed already. When such a customer is migrating to `version 2.0`, they can do either of the following,

1. For every migration, uninstall Nosto, migrate your files and then reinstall Nosto from within the Nosto app
2. Migrate everything excluding Nosto content

The reason is Nosto uses unique logic for injecting contents into Shopify themes. With the introduction of structureless json shema, it became little challenging to isolate Nosto content for easy installation or uninstallation. In case if the Nosto contents are copied over during manual migration, this could result in duplication on Nosto sections on screen. In those scenarios, we will be able to identify and mitiagte the issue by removing the duplicated contents from liquid templates. We can avoid this issue, if we follow the points above and use Nosto app to install or uninstall Nosto contents.
In future, we will introduce other options to better handle this situation

## Limitations

As per [this](https://community.shopify.com/c/shopify-design/page-s-section-limit-on-2-0/m-p/1239775/highlight/true#M321332) discussion on Shopify Community, shopify has a limitation on number of sections allowed on homepage. Because of this, when `index.json` is configured with  20 sections, will cause issues during Nosto installation and eventually cause issues with rendering front page placements. In such scenario, modifying `templates/index.json` to use 19 sections and reinstalling Nosto will fix the issue. This approach has to be followed until either Shopify increases this limitation or removes the limitation altogether. 