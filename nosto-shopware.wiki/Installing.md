The preferred way of installing the plugin is through the Shopware Community Store. It can, however, also be installed as a local package or directly from the GitHub repository if needed.

For development purposes it is recommended to use the repository approach, as you can then easily keep track of any changes while having the plugin installed in Shopware.

## Community Store

The plugin can be automatically downloaded and installed from within Shopware, if you have connected your Shopware account to the installation. The plugin is found under the `Customer account + personalization` section in the Plugin Manager, or by searching for "nosto". If you can't find it, you can also manually download it from the [Community Store](http://store.shopware.com/en/nosto32157561647/nosto-personalization-for-shopware.html).

Once you've found the plugin, simply click `Download now` button on the plugin page and follow the instructions to activate the plugin.

## Local

The plugin can also be installed as a "local" plugin by uploading the plugin archive file manually in the Shopware Plugin Manager, or by extracting it directly into the `SHOPWARE/engine/Shopware/Plugins/Local/Frontend/` folder inside Shopware. The plugin archive file can be obtained from the projects [releases](https://github.com/Nosto/nosto-shopware-plugin/releases) page on GitHub.

**NOTE:** download the latest release package called `NostoTagging-x.x.x.zip`, and NOT the "source code". The release package contains the needed dependencies which the source does not.

After this, the plugin can be activated in the Shopware Plugin Manager.

## Repository

For development purposes the plugin can be installed directly from the GitHub repository by cloning the project into the `/engine/Shopware/Plugins/Local/Frontend/NostoTagging` directory of the Shopware installation. For the plugin to work, it's dependencies need to be installed. For this we recommend using [composer](https://getcomposer.org/), which is a dependency manager for PHP. By executing composer install in the plugins root folder, the dependencies will be automatically fetched and installed in a vendor folder relative to the plugin root directory.

After this, the plugin can be activated in the Shopware Plugin Manager.