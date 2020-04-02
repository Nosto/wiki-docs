It's recommended to install the plugin from the repository when developing, please read the [install section](Installing#repository).

When developing or testing the module it may be useful to connect it to your local Nosto environment or the staging environment instead of the live one. This can be done by modifying a "Dot Env" (.env) file in the Nosto SDK that the module uses. After having installed the module in Shopware, copy `SHOPWARE/engine/Shopware/Plugins/Local/Frontend/NostoTagging/vendor/nosto/php-sdk/.env.example` to `SHOPWARE/engine/Shopware/Plugins/Local/Frontend/NostoTagging/vendor/nosto/php-sdk/.env` and modify the parameters.

* `NOSTO_SERVER_URL` is the Nosto URL used in the storefront 
* `NOSTO_API_BASE_URL` is the base URL for all API calls to Nosto
* `NOSTO_OAUTH_BASE_URL` is the base URL for connecting Nosto accounts through OAuth
* `NOSTO_WEB_HOOK_BASE_URL` is the base URL for Nosto web hooks
* `NOSTO_IFRAME_ORIGIN_REGEXP` is a regexp for validating window.postMessage() event origin dispatched by the account configuration iframe (not in use in this module)

The sample `.env` file can viewed at https://github.com/Nosto/php-sdk/blob/master/.env.example

Note that you can only have one .env file at a time, and if you wish to switch between environments you can copy them into `.env.[environment]` files. This way you can switch the environment by replacing the .env with the correct .env.[environment] file.

**NOTE:** when modifying the backend ExtJS app, you need to either disable the Shopware cache or clear it every time you want to test your new changes.

Coding Standards
---

The extension follows the Zend coding standards that come bundled with PHP Code Sniffer. The following warnings can be ignored or disabled:

  1. Warnings about the line length
  2. Warnings about tab indents (the project uses tab indents)

You can run Code Sniffer using `phpcs --standard=Zend --extensions=php --ignore=vendor .`