# Uninstalling

Currently the only way of uninstalling the module is via the command line. You can uninstall the module by running following commands inside Magento's installation base directory.

**Note:** Before running the `bin/magento` command make sure the file is executable.

```text
bin/magento module:uninstall Nosto_Msi
bin/magento setup:upgrade
bin/magento cache:clean
bin/magento setup:di:compile
```

After the extension is uninstalled you still need to remove the package from your Magento dependencies using [Composer](https://getcomposer.org/). If you don't have composer installed yet you can get it by following [these instructions](https://getcomposer.org/doc/00-intro.md). It's recommended to install composer globally. Once you have composer installed you can uninstall the module \(nosto/module-nostotagging-msi\).

```text
composer remove nosto/module-nostotagging-msi
```

