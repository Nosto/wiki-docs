# Console Commands

It is possible to use a console command to reconnect or remove the account. This process can be handy when your deployments are automated.

![3.0.0](https://img.shields.io/badge/nosto-3.0.0-green.svg)

## Getting tokens from the Nosto Admin Panel.

Log-in into [Nosto Admin](https://my.nosto.com) with your already existing account, select the store you want to reconnect and head to `Settings -> Authentication Tokens`.  
 To obtain the tokens, just hit the decrypt button. ![cropper](https://user-images.githubusercontent.com/2778820/44570846-cf96f300-a787-11e8-952e-0fc1950ea77e.png)

## Getting Store View Scope Code From Magento's Backend

Since you need a different Nosto account for each store view, you need to get the store scope code. You can find in: `Stores -> All stores`

![image](https://user-images.githubusercontent.com/2778820/44571925-f73b8a80-a78a-11e8-996c-6d0dbb998459.png)

Click on your store view:

![stores\_\_\_settings\_\_\_stores\_\_\_magento\_admin](https://user-images.githubusercontent.com/2778820/44571988-26ea9280-a78b-11e8-8e68-07f0de17eb07.png)

Copy the value from de `code` field

![stores\_\_\_default\_store\_view\_\_\_settings\_\_\_stores\_\_\_magento\_admin](https://user-images.githubusercontent.com/2778820/44572053-500b2300-a78b-11e8-993f-d311d233856b.png)

## Using the Console Command to Reconnect Your Existing Nosto Account

### Using the interactive mode

* Open the terminal and head to you Magento installation path directory

  You can run:

  `bin/magento nosto:account:connect` and enter the interactive mode, where you will input the required tokens.

  ![image](https://user-images.githubusercontent.com/2778820/44572711-5d291180-a78d-11e8-99c7-f6468621156a.png)

### Using the non-interactive mode

In order to use this command with non-interactive installation scripts, you can also pass all the parameters via the command line arguments.

You can use the help from the command to figure out all the necessary parameters: ![image](https://user-images.githubusercontent.com/2778820/44572987-1851aa80-a78e-11e8-8690-284a3a222670.png)

#### Example of non-interactive command:

```bash
bin/magento nosto:account:connect \
--account-id=NOSTO_ACCOUNT_NAME \
--sso_token=SSO_TOKEN \
--products_token=PRODUCTS_TOKEN \
--settings_token=SETTINGS_TOKEN \
--rates_token=RATES_TOKEN \
--apps_token=APPS_TOKEN \
--scope-code=STORE_VIEW_SCOPE_CODE --no-interaction
```

![image](https://user-images.githubusercontent.com/2778820/44573107-741c3380-a78e-11e8-909f-19620a62bf51.png)

## Using the Console Command to Remove Nosto account from your store view

This command is used to disconnect an existing account from your store.

### Using the interactive mode

* Open the terminal and head to you Magento installation path directory

  You can run:

  `bin/magento nosto:account:remove` and enter the interactive mode, where you will input the store view scope code.

  ![image](https://user-images.githubusercontent.com/44775916/49924280-8a293d80-febe-11e8-8230-bf816efc1784.png)

### Using the non-interactive mode

In order to use this command with non-interactive installation scripts, you can also pass the parameter via the command line arguments.

#### Example of non-interactive command:

```bash
bin/magento nosto:account:remove \
--scope-code=STORE_VIEW_SCOPE_CODE --no-interaction
```

![image](https://user-images.githubusercontent.com/44775916/49924701-9a8de800-febf-11e8-8789-6ab37d4a8f6c.png)

