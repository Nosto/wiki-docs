When developing or testing an extension using the SDK it may be useful to connect it to your local Nosto environment or the staging environment instead of the live one. This can be done by modifying a "Dot Env" (.env) file in the Nosto SDK that the extension uses. For example for Magento extension, copy `MAGENTO/lib/nosto/php-sdk/.env.example` to `MAGENTO/lib/nosto/php-sdk/.env` and modify the parameters.

* `NOSTO_SERVER_URL` is the Nosto url used in the store front 
* `NOSTO_API_BASE_URL` is the base url for all API calls to Nosto
* `NOSTO_OAUTH_BASE_URL` is the base url for connecting Nosto accounts through OAuth
* `NOSTO_WEB_HOOK_BASE_URL` is the base url for Nosto web hooks
* `NOSTO_IFRAME_ORIGIN_REGEXP` is a regexp for validating window.postMessage() event origin dispatched by the account configuration iframe 

The sample `.env` file can viewed at https://github.com/Nosto/php-sdk/blob/master/.env.example

Note that you can only have one .env file at a time, and if you wish to switch between environments you can copy them into `.env.[environment]` files. This way you can switch the environment by replacing the .env with the correct .env.[environment] file.


### Coding Standards

The extension follows a customised version the PSR-2 coding standards that come bundled with PHP Code Sniffer. A custom ruleset is bundles with the code. The following warnings are supressed:

* Warnings about missing namespaces

#### Install Codesniffer

First `cd` into the root directory.

Then install Codesniffer via composer:

```bash
curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar
php phpcs.phar install
```

#### Running Codesniffer

You can run Code Sniffer using 

```bash
phpcs --standard=ruleset.xml --extensions=php --ignore=lib --ignore=_data --ignore=_support --ignore=tests .`
```


### Testing

The SDK is unit tested with Codeception (http://codeception.com/).
API and OAuth2 requests are tested using api-mock server (https://www.npmjs.com/package/api-mock) running on Node.

#### Install Codeception & API-mock

First cd into the root directory.

Then install Codeception via composer:

```bash
php composer.phar install
```

And then install Node (http://nodejs.org/) and the npm package manager (https://www.npmjs.com/). Node 0.12 is required by api-mock so you might need to install nvm (http://nvm.sh) and set node version to 0.12 (```nvm install 0.12```) before running api-mock . After that you can install the api-mock server via npm:

```bash
npm install -g api-mock
```

#### Running tests

First cd into the root directory.

Then start the api-mock server with the API blueprint:

```bash
api-mock tests/api-blueprint.md
```

When running the tests for the first time you must generate the helper classes by running:

```bash
vendor/bin/codecept build
```
Then in another window run the tests:

```bash
vendor/bin/codecept run
```