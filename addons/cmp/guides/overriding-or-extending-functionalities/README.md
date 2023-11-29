# Overriding or Extending Functionalities

In order to extend Nosto, we can create a new Magento 2 module that will override the `vendor` files.

## Directory Structure

Creating an override requires a few files to be created. Follow the guide below and simply copy-paste the content into the specified locations. This will form the scaffold of your new override.

### 1. The module config `app/code/My/Nosto/etc/module.xml`

```markup
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Module/etc/module.xsd">
    <module name="My_Nosto" setup_version="1.0.0" />
</config>
```

### 2. The module registration `app/code/My/Nosto/registration.php`

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'My_Nosto',
    __DIR__
);
```

### 3. The composer `app/code/My/Nosto/composer.json`

```javascript
{
    "name": "my/nosto-extension",
    "description": "Custom extension for altering product data",
    "type": "magento2-module",
    "version": "1.0.0",
    "license": [
        "OSL-3.0"
    ],
    "minimum-stability": "dev",
    "require": {
        "php": ">=5.6.0",
        "nosto/module-nostotagging-cmp": "@stable"
    },
    "autoload": {
        "psr-4": {
          "My\\Nosto\\": ""
        },
        "files": [ "registration.php" ]
    }
}
```

You now should have this directory structure:

```bash
app
├── code
│   └── My
│       └── Nosto
│           ├── composer.json
│           ├── etc
│           │   └── module.xml
│           ├── registration.php
```

## Enabling the plugin

Before the module begins to work, you will need to explicitly enable the module by running a series of commands.

```text
bin/magento module:enable --clear-static-content My_Nosto
bin/magento setup:upgrade
```

## Disabling the plugin

In order to disable the module without removing the code, you will need to run a series of commands.

```text
bin/magento module:disable --clear-static-content My_Nosto
bin/magento setup:upgrade
```

The changes you make will be taken into use instantly both in the shop.

## Verifying

Once you have overridden the associated model and customised whatever fields you may need, you should verify that it, in fact, working as expected.

A simple way to verify that the changes are working would be to log in as a customer and view any page with the Nosto debug-mode enabled. The debug mode can be enabled by adding the query parameter `nostodebug=true` to the end of any URL. This will cause a helpful debug toolbar to appear where you can view the tagged data on the page. For more information on the debug-toolbar, please refer to this guide titled [Nosto Debug Toolbar](https://support.nosto.com/get-started/nosto-debug-toolbar/) in our Support Center.

**NOTE:** Please note that in order to verify the changes using the debug-toolbar, you must have a Nosto account for the given store.

## Using the module in production environment

Before using your custom module in production environment we strongly recommend that you follow the instructions how to package a Magento 2 module. If you are familiar with creating packages for composer, creating a package for M2 is simple. You can find the instructions from [here](http://devdocs.magento.com/guides/v2.0/extension-dev-guide/package/package_module.html)
