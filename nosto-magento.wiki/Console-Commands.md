It is possible to use a console command to reconnect or remove the account. This process can be handy when your deployments are automated.

![3.8.0](https://img.shields.io/badge/nosto-3.8.0-red.svg)

## Getting tokens from the Nosto Admin Panel.

Log-in into [Nosto Admin](https://my.nosto.com) with your already existing account, select the store you want to reconnect and head to `Settings -> Authentication Tokens`.<br>
To obtain the tokens, just hit the decrypt button.
<img width="1669" alt="cropper" src="https://user-images.githubusercontent.com/2778820/44570846-cf96f300-a787-11e8-952e-0fc1950ea77e.png">

## Getting Store View Scope Code From Magento's Backend
Since you need a different Nosto account for each store view, you need to get the store scope code. You can find in:
`System -> Manage stores`

![stores___system___magento_admin_and_editing_reconnecting_nosto_account_via_cli_command_ _nosto_nosto-magento2_wiki](https://user-images.githubusercontent.com/2778820/44987528-2d012000-af90-11e8-907d-710ecca01db9.png)

You can find the store code under the store view link. If for any reason Magento do not show this information, you can also click in the storeview and lookup in the `code` field

![english___stores___system___magento_admin](https://user-images.githubusercontent.com/2778820/44987581-57eb7400-af90-11e8-99d2-74fc28c521e1.png)


## Using the Console Command to Reconnect Your Existing Nosto Account
The script file should be available in the `%nosto_installation_root%/shell/reconnect.php`.<br>

If you have installed the extension through modman, the installation root directory should be under `%magento_root%/.modman/nosto-magento`. <br>

If you have installed via Magento Connect or via package upload, the extension should be in the ` app/code/community/Nosto/Tagging` directory. <br>

In order to use this command with non-interactive installation scripts, you should pass all the parameters via command line arguments.

You can use the help from the command to figure out all the necessary parameters:
![image](https://user-images.githubusercontent.com/2778820/44987768-07c0e180-af91-11e8-8743-aa4636b82f7b.png)


#### Command Example:
```bash
php -f app/code/community/Nosto/Tagging/shell/reconnect.php -- \
--account-id NOSTO_ACCOUNT_NAME \
--products_token PRODUCTS_TOKEN \
--sso_token SSO_TOKEN \
--settings_token SETTINGS_TOKEN \
--rates_token EXCHANGE_RATES_TOKEN \
--scope-code STORE_VIEW_SCOPE_CODE \
--override
```
![image](https://user-images.githubusercontent.com/2778820/44988170-6e92ca80-af92-11e8-9f38-aacd73f7a4f9.png)


## Using the Console Command to Remove Your Nosto Account
This command is used to disconnect an existing account from your store.

The script file should be available in the `%nosto_installation_root%/shell/remove.php`.<br>

If you have installed the extension through modman, the installation root directory should be under `%magento_root%/.modman/nosto-magento`. <br>

If you have installed via Magento Connect or via package upload, the extension should be in the ` app/code/community/Nosto/Tagging` directory. <br>

In order to use this command with non-interactive installation scripts, you should pass all the parameters via command line arguments.

![image](https://user-images.githubusercontent.com/44775916/49878267-d24d4f00-fe2f-11e8-85bc-d0395caed695.png)

#### Command Example:
```bash
php -f app/code/community/Nosto/Tagging/shell/remove.php -- \                                  
--scope-code STORE_VIEW_SCOPE_CODE
```
![image](https://user-images.githubusercontent.com/44775916/49878573-949cf600-fe30-11e8-85a9-981cd4b74b9a.png)
