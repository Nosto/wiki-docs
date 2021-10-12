# Installing

## PHP Requirements

The Nosto Magento 2 extension requires at least PHP version 7.0.0.

## Installing the extension

Currently the only way of installing the extension is via [Composer](https://getcomposer.org/). If you don't have composer installed yet you can get it by following [these instructions](https://getcomposer.org/doc/00-intro.md). It's recommended to install composer globally. You will also need public key and private key from Magento Marketplace or Magento Connect in order to install packages to Magento 2 via Composer. Please follow these instructions to get public key and private key [http://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html](http://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html). Once you have composer installed you can install Nosto extension \(nosto/module-nostotagging\).

```text
composer require --no-update nosto/module-nostotagging:"@stable" && composer update --no-dev
```

After the packages are installed you still need to enable the module by running following commands inside Magento's installation base directory.

**Note:** Before running the `bin/magento` command make sure the file is executable.

```text
bin/magento module:enable --clear-static-content Nosto_Tagging
bin/magento setup:upgrade
bin/magento cache:clean
bin/magento setup:di:compile
```

You might need to change file permissions or ownership of the generated files after the installation.

After running the commands above you can login to the store admin. You will find Nosto -link under Marketing section.

#### Magento's repo does not contain the latest release
In case you are pulling the dependencies from Magento's repository `repo.magento.com` and you encounter an error where the latest release is not present, 
you can [exclude](https://getcomposer.org/doc/articles/repository-priorities.md#filtering-packages) Nosto packages and instead pull them from the default repository `https://repo.packagist.org/`. 
```json
"repositories": [
    {
        "type": "composer",
        "url": "https://repo.magento.com/",
        "exclude": ["nosto/*"]
    }
]
```

## Indexer mode

We strongly recommend that you set the mode for Nosto indexer\(s\) to be "Update by schedule". This is important especially with large product catalogs and / or when using multiple store views. Read more about indexer performance and optimization [here](features/indexer/)

