# Installing

## Compatibility with Magento apps (search engines) <a href="#compatibility-for-magento-apps" id="compatibility-for-magento-apps"></a>

Magento is compatible with Elastic Search/Suite. We do not support MySQL as a search engine:

| Magento Apps   | Compatibility  |
|----------------|--------------- |
| Elastic Search | Compatible     |
| Elastic Suite  | Not compatible |
| MySQL          | Not compatible |

## PHP Requirements

The Nosto Magento 2 module requires at least PHP version &gt;= 7.0.0 and Nosto's Magento 2 module &gt;= 4.0.0

## Installing the extension

> Link to GitHub repository: https://github.com/Nosto/nosto-magento2-cmp

Currently, the only way of installing the module is via [Composer](https://getcomposer.org/). If you don't have composer installed you can get it by following [these instructions](https://getcomposer.org/doc/00-intro.md). It's recommended to install composer globally. You will also need public key and private key from Magento Marketplace or Magento Connect in order to install packages to Magento 2 via Composer. Please follow these instructions to get public key and private key [http://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html](http://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html). Once you have composer installed you can install Nosto extension \(nosto/module-nostotagging-cmp\).

```text
composer require --no-update nosto/module-nostotagging-cmp:"@stable" && composer update --no-dev
```

```text
bin/magento module:enable --clear-static-content Nosto_Cmp
bin/magento setup:upgrade
bin/magento cache:clean
bin/magento setup:di:compile
```

You might need to change file permissions or ownership of the generated files after the installation.

After running the commands above, you can log-in to the store admin. You will find settings for `Category personalization and merchandizing` module under Stores &gt; Configuration &gt; Services &gt; Nosto CMP.

