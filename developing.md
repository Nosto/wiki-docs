# Developing & Contributing

It's recommended to install the module from the repository when developing, please read the [install from repository section](installing.md#installing-via-a-repository-github).

When developing or testing the module it may be useful to connect it to your local Nosto environment or the the staging environment instead of the live one. This can be done by modying a "Dot Env" \(.env\) file in the Nosto SDK that the module uses. After having installed the module in PrestaShop, copy `PRESTASHOP/modules/nostotagging/libs/nosto/php-sdk/.env.example` to `PRESTASHOP/modules/nostotagging/libs/nosto/php-sdk/.env` and modify the parameters.

* `NOSTO_SERVER_URL` is the Nosto url used in the store front 
* `NOSTO_API_BASE_URL` is the base url for all API calls to Nosto
* `NOSTO_OAUTH_BASE_URL` is the base url for connecting Nosto accounts through OAuth
* `NOSTO_WEB_HOOK_BASE_URL` is the base url for Nosto web hooks
* `NOSTO_IFRAME_ORIGIN_REGEXP` is a regexp for validating window.postMessage\(\) event origin dispatched by the account configuration iframe \(not in use in this module\)

The sample `.env` file can viewed [here](https://github.com/Nosto/php-sdk/blob/master/src/.env)

Note that you can only have one .env file at a time, and if you wish to switch between environments you can copy them into `.env.[environment]` files. This way you can switch the environment by replacing the .env with the correct .env.\[environment\] file.

## Running PHP Code Sniffer

In order to meet Prestashop's module validation rules, you must run the PHP Code Sniffer and no errors or warnings can be found. The repo has a custom rules defined in `ruleset.xml`. You must define the `ruleset.xml`as the standard when running phpcs. In case you don't have phpcs installed, you can get it from here: [https://github.com/squizlabs/PHP\_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)

Once installed go to the root directory and run

```bash
phpcs --standard=ruleset.xml -v .
```

## Development on Prestashop 1.7

Run "composer install --no-dev" to install dependencies excluding development required dependencies

## Releasing and Packaging

When doing new releases of the module, you need to check the following:

1. The version number
   * Always change the version number when doing a new release
   * The version numbering follows the semantic versioning standard \([http://semver.org/](http://semver.org/)\)
   * The version number is located in the main module file and the config.xml file, both in the modules root directory 
   * Always create an entry in the CHANGELOG.md file for each new version
2. Update scripts
   * If the changes to the module require database changes, new custom hooks, structural or content, then an update script must be created
   * PrestaShop uses this script to do necessary changes when updating between versions
   * Place the script in `upgrade/upgrade-x.x.x.php` where x.x.x corresponds to the new version number

In order to publish the new release to PrestaShop addons, you need to create a pull-request to the `PrestaShop/nostotagging` repository, which this repository is a fork of. Always create the pull-request from Nosto:master =&gt; PrestaShop:master. PrestaShop will then review the release and publish it within a day or two. Remember to give a good explanation of the things that have changed in the pull-request comment field. It will help PrestaShop when reviewing.

### Packaging the module

Packaging the module is done with [Phing](https://www.phing.info/). You will get Phing automatically when running composer without excluding the dev depencies. In other words, just run `composer install` in the root directory. The only parameters you must give to phing command are the version of the plug-in and the path to global composer \(where PHP Code sniffer can be found\). Please note that you must have [PHP Code Sniffer](https://github.com/squizlabs/PHP_CodeSniffer) and [PHP Mess Detector](https://phpmd.org/) installed and available for Phing.

After phing is installed via composer, you must run

```bash
./libs/bin/phing -verbose -Dversion=X.Y.X -DcomposerDir=/path/to/composer
```

For example for version 2.7.1 the command would be

```bash
./libs/bin/phing -verbose -Dversion=2.7.1 -DcomposerDir=/home/nostodev/.composer
```

After successfully running the phing you will find a ready to install package from `$root_dir/build/package`

