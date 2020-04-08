# Developing & Contributing

When developing or testing the extension it may be useful to connect it to your local Nosto environment or the the staging environment instead of the live one. This can be done by modying a "Dot Env" \(.env\) file in the Nosto SDK that the extension uses. After having installed the extension in Magento, copy `MAGENTO/lib/nosto/php-sdk/.env.example` to `MAGENTO/lib/nosto/php-sdk/.env` and modify the parameters.

* `NOSTO_SERVER_URL` is the Nosto url used in the store front 
* `NOSTO_API_BASE_URL` is the base url for all API calls to Nosto
* `NOSTO_OAUTH_BASE_URL` is the base url for connecting Nosto accounts through OAuth
* `NOSTO_WEB_HOOK_BASE_URL` is the base url for Nosto web hooks
* `NOSTO_IFRAME_ORIGIN_REGEXP` is a regexp for validating window.postMessage\(\) event origin dispatched by the account configuration iframe 

The sample `.env` file can viewed [here](https://github.com/Nosto/php-sdk/blob/master/src/.env)

Note that you can only have one .env file at a time, and if you wish to switch between environments you can copy them into `.env.[environment]` files. This way you can switch the environment by replacing the .env with the correct .env.\[environment\] file.

Resources

* [http://devdocs.magento.com/guides/m1x/magefordev/mage-for-dev-1.html](http://devdocs.magento.com/guides/m1x/magefordev/mage-for-dev-1.html)

## Javascript development

In order to make changes to javascript files you must first modify the layout files so that the javascript sources are pointing to the not minified versions of the files. If you for example need to make changes to the iframe handler \(`js/nosto/iframeHandler.min.js`\) do the following modifications to `app/design/adminhtml/default/default/layout/nostotagging.xml`.

```markup
<config>
    <!-- Nosto main menu link page -->
    <adminhtml_nosto_index>
        <!-- Add JavaScript file -->
        <reference name="head">
            <action method="addJs"><script>nosto/iframeResizer.min.js</script></action>
            <!-- ToDo - change back after minifying -->
            <!--<action method="addJs"><script>nosto/iframeHandler.min.js</script></action>-->
            <action method="addJs"><script>nosto-src/iframeHandler.js</script></action>
        </reference>
```

Make sure the `nosto-src` directory is symlinked from `$mage_root/js/` correctly to `js/nosto-src` in Nosto extension.

Once you have made the changes to javascript file the files must be minified using [Grunt](http://gruntjs.com/). Simply run `grunt uglify` in the extension root folder and change you layout files again to point to the minified versions.

Please note that currently only the `iframeHandler.js` can be minified with grunt.

## Packaging

The recommended way of packaging the extension is by using [Phing](https://www.phing.info/). Phing uses [Magazine](https://github.com/mridang/magazine) to create a Magento Connect compatible package. Using Phing is simple.

First, uppdate your dependencies by running:

```text
composer install
```

The run following command in the extension's root directory to package the extension.

```bash
./lib/bin/phing -verbose -Dversion=X.Y.Z
```

The version parameter must match the version number you have in the `app/code/community/Nosto/Tagging/etc/config.xml` and in the `magazine.json`.

## Releasing

To release a new version of the extension to the Magento Connect Marketplace, you do the following:

Log in to Magento Connect and click on "Developers" in the left-hand menu. Under the "Manage Extensions" section find the extension and click "Edit". Click the "Versions" tab at the top of the page and then click the "Add new version" button at the bottom of the page. Fill in the release details:

* Version Number: Enter exactly the same version number as in the extension you are releasing \(`config.xml` file\)
* Version Stability: "Stable"
* Release Notes Title: Any title.
* Release Notes: Any releases notes. For example, the changelog.
* Show on Frontend: "Yes"

Select all versions from 1.6 to the newest supported version from the "Community" section and select all versions from 1.11 to the newest supported version under the "Enterprise" section. Click the "Continue to upload" button, choose the .tgz release package you want to release and then click "Upload and save".

