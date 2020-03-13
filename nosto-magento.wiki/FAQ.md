## How do I view extension errors?

When the extension encounters an error, it logs it in a separate error log file in Magento. This file can be found under `var/log/nostotagging.log`. Only errors the extension can recover from are logged here, and PHP fatal errors, for example, are logged in the default Magento error logs located in the same directory. This is because these kind of errors stop the execution of the code and Magento's error handler takes over. Fatal errors are only thrown by critical bugs in the extension, which are very rare.

Hint: by installing the free [Log Viewer](http://www.magentocommerce.com/magento-connect/log-viewer-1.html) extension, you can easily view all logs in the admin section of Magento.

## Why am I seeing "Fatal error: Class 'Nosto_Tagging_Model_Resource_Setup' not found" after installing the extension?

If you encounter an error like this, it is most likely due to having the [Magento Compiler](https://kb.magenting.com/content/24/81/en/disable-magento-compiler.html) enabled while installing the extension. Magento does not support having it enabled while making modifications to the installation, such as installing new extensions. You should:

* Disable the compiler
* Re-install the extension
* Empty the compiler cache and enable the compiler
* Re-compile

## Why can't I see any Nosto related changes (tagging & scripts) in my page source?
Most likely you need to clear the Magento cache. See next section how to clear the cache.

## How can I clear my Magento cache?

If you've created an account but are still unable to see the Nosto related markup and scripts in your page source, this would probably be caused due to Magento's page caching. When you make any modifications to your Magento store, they will not appear immediately unless you clear the cache. The cache can be cleared by following these steps:

![system-cache-management](https://cloud.githubusercontent.com/assets/327432/9107139/b520d112-3c2d-11e5-83fb-596a31f54d0b.png)

1. Open Magento admin panel
2. Go to "System" > "Cache Management"
3. Click the "Flush Magento Cache" and the "Flush Cache Storage" buttons


## Why does Nosto report my products as in stock even though the product is out of stock?

This is commonly caused by having the skip-saleable-check flag enabled. Please note this flag is disabled by default. The skip-saleable-check configuration can be queried using:

`Mage::helper('catalog/product')->getSkipSaleableCheck()`

In order to disable this, please run:

`Mage::helper('catalog/product')->setSkipSaleableCheck(false)`


## How can I delete all the Nosto configuration from the DB?

Use the following SQL to remove all the configuration:

`DELETE FROM core_config_data WHERE path LIKE 'nosto_tagging/%';`

## How can I create an account if my admin is non-functional due to JS errors?

The Nosto IFrame heavily relies on cross-domain communication using post/receive message handlers. In the event that
your admin isn't working due to JS errors, you can use the following script as a temporary workaround to install
Nosto account.

Navigate to the Nosto tab in Magento store admin and choose the store-view you want to create Nosto for and paste
the script function definition below into your browser's console.

**Please exercise caution when using this script**

```javascript
function installNosto(path, params) {
    var form = document.createElement("form");
    form.setAttribute("method", "post");
    form.setAttribute("action", path);

    for(var key in params) {
        if(params.hasOwnProperty(key)) {
            var hiddenField = document.createElement("input");
            hiddenField.setAttribute("type", "hidden");
            hiddenField.setAttribute("name", key);
            hiddenField.setAttribute("value", params[key]);

            form.appendChild(hiddenField);
         }
    }

    document.body.appendChild(form);
    form.submit();
}
```
Once you have defined the above function you need to figure out the URL for creating new Nosto account. The easiest way to do this is
by viewing the page source of the Magento's Nosto page and finding string `createAccount:`.
The uninstall URL is the value of the attribute `createAccount` and it should be something like this
`https://your-magento-domain/index.php/admin/nosto/createAccount/store/1/isAjax/1/key/a241407ffa1aerwe534trtgrbff/`. Copy
this URL and call `installNosto` in your browser's console with the URL as the first parameter. You also need to define the `form_key` and `email` parameters. Example can be found below.

```javascript
params = {
    'email':'your.email@yourdomain.tld',
    'form_key': window.FORM_KEY
};
installNosto('https://your-magento-domain/index.php/admin/nosto/removeAccount/store/1/isAjax/1/key/a241407ffa1aerwe534trtgrbff/', params);
```

You should see JSON formatted response. Navigate back to the Magento's Nosto settings and reload page is Nosto is not installed.

## How can I uninstall Nosto account if my admin is non-functional due to JS errors?

The Nosto IFrame heavily relies on cross-domain communication using post/receive message handlers. In the event that
your admin isn't working due to JS errors, you can use the following script as a temporary workaround to uninstall
Nosto account.

Navigate to the Nosto tab in Magento store admin and choose the store-view you want to create Nosto for and paste
the script function definition below into your browser's console.

**Please exercise caution when using this script**

```javascript
function uninstallNosto(url) {
    var options = {
        method: "POST",
        async: false,
        data: {'form_key' : window.FORM_KEY}
    };
    var oReq = new XMLHttpRequest;
    oReq.open(options.method, decodeURIComponent(url), options.async);
    oReq.setRequestHeader("X-Requested-With", "XMLHttpRequest");
    oReq.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    var queryString = "";
    for (var key in options.data) {
        if (options.data.hasOwnProperty(key)) {
            if (queryString !== "") {
                queryString += "&"
            }
            queryString += encodeURIComponent(key) + "=" + encodeURIComponent(options.data[key])
        }
    }
    oReq.send(queryString);
    location.reload();
}
```
Once you have defined the above function you need to figure out the URL for uninstalling Nosto account. The easiest way to do this is
by viewing the page source of the Magento's Nosto page and finding string `deleteAccount:`.
The uninstall URL is the value of the attribute `deleteAccount` and it should be something like this
`https://your-magento-domain/index.php/admin/nosto/removeAccount/store/1/isAjax/1/key/a241407ffa1aerwe534trtgrbff/`. Copy
this URL and call `unistallNosto` in your browser's console with the URL as a parameter. Example can be found below.

```javascript
uninstallNosto('https://your-magento-domain/index.php/admin/nosto/removeAccount/store/1/isAjax/1/key/a241407ffa1aerwe534trtgrbff/')
```

If the Nosto account doesn't seem to be removed please reload the page.

## How does the extension choose the product image?

The extension, by default, uses the "Base Image". You can select a different image version to be used by visiting the advanced configuration of the extension. You can navigate there by choosing "Configuration" from the "System" menu in the menu bar and then choosing "Nosto", under the "Services" section on the left panel. This option is only available on a store-view level.

![](https://cloud.githubusercontent.com/assets/327432/19802512/bf6b9526-9d0c-11e6-8555-c076aef5ab21.png)


## How does the extension behave when I use FPC (caching)?

If you are using Magento EE and Magento EE's caching mechanism then the extension ships with hole-punch out of the box. The hole-punching rules for Magento EE's cache can be found here: https://github.com/Nosto/nosto-magento/blob/develop/app/code/community/Nosto/Tagging/etc/cache.xml

If you are using Magento CE or a third-party caching mechanism. It is important that you exclude certain Nosto blocks from caching. The extension injects some hidden markup into the page to denote the currently logged-in customer (using a block named `nosto.customer`) and the current customer's shopping cart contents (using a block named `nosto.cart`). These can be found here:

https://github.com/Nosto/nosto-magento/blob/develop/app/design/frontend/base/default/layout/nostotagging.xml#L56
https://github.com/Nosto/nosto-magento/blob/develop/app/design/frontend/base/default/layout/nostotagging.xml#L51

These two blocks must be excluded from the FPC. Without exclusion, the previous customer's personal information, and cart-contents will be displayed to the wrong customer and Nosto will leak sensitive private browsing information across sessions. 

The extension also injects some Javascript and some recommendation elements across different parts of the website. The Javascript populates the recommendation elements on the client side (browser) and therefore these are not prone to caching issues.

Due to the huge array of FPC extensions available, it is not possible for the extension to denote which blocks must be hole-punched and you must read your FPC extension's documentation to configure and exclude the aforementioned blocks.


## How is the price and list price calculated?

The extension always uses the lowest price available when it a bundled product, a grouped product or a configurable product. It does not factor in availability of the child variations when calculating the minimum price.

For example, a shirt has three variations - a red shirt for $100, a blue shirt for $200 and a green shirt for $300. The price of the product would always be $100 even if the red shirt was out of stock.

For simple and virtual products, we take the product's price as there are no child variations.

## How hidden categories are handled in Nosto tagging?

Hidden categories are not tagged in Nosto product tagging. If a hidden category has subcategories the subcategories are omitted as well. 

For example, if a product belongs category `Accessories/Women/Sunglasses/Police` and `Sunglasses` is hidden, both `Sunglasses` and `Police` are omitted. Thus, product category in Nosto tagging in this example would be `Accessories/Women`.

## Why do I get this error `Parse error: syntax error, unexpected T_USE, expecting T_FUNCTION`?

Both Nosto's extension and Magento require PHP 5.4+ to work. Our code uses traits which are only supported from PHP 5.4 onwards. We recommend you upgrade to PHP 5.4+ in order for the extension to function.

## How do I remove the `___store` parameter from the URLs?

In order to remove the `___store` parameter, you will need to enable clean-urls in the advanced configuration. Please see the [URL configuration](Configuring#url-configuration) section in our configuration guide.

## Why is the Nosto script not being rendered and how can I fix it?

The Nosto script, the Javascript stub are loaded in the `head` of your theme. It is possible that your theme is missing the default Magento layout handle for the `head`.

You need to edit the base layout definition file for your store's default theme and ensure that the line `<?php echo $this->getChildHtml('head') ?>` is correctly placed somewhere between the HTML `<head>...</head>` tags. Below is an excerpt from the default Magento theme file name `1column.phtml`

```php
<?php
/**
 * Template for Mage_Page_Block_Html
 */
?>

<!DOCTYPE html>

<!--[if lt IE 7 ]> <html lang="<?php echo $this->getLang(); ?>" id="top" class="no-js ie6"> <![endif]-->
<!--[if IE 7 ]>    <html lang="<?php echo $this->getLang(); ?>" id="top" class="no-js ie7"> <![endif]-->
<!--[if IE 8 ]>    <html lang="<?php echo $this->getLang(); ?>" id="top" class="no-js ie8"> <![endif]-->
<!--[if IE 9 ]>    <html lang="<?php echo $this->getLang(); ?>" id="top" class="no-js ie9"> <![endif]-->
<!--[if (gt IE 9)|!(IE)]><!--> <html lang="<?php echo $this->getLang(); ?>" id="top" class="no-js"> <!--<![endif]-->

<head>
<?php echo $this->getChildHtml('head') ?>
</head>
<body<?php echo $this->getBodyClass()?' class="'.$this->getBodyClass().'"':'' ?>>
<?php echo $this->getChildHtml('after_body_start') ?>
<div class="wrapper">
```

Attempting to add the script via other methods may result in broken upgrades.


## Why do I get errors such as `'Nosto_Request_Http_HttpRequest' not found` after installing or upgrading?

This is probably due to a bad installation procedure. If you have downloaded the `.tgz` file, unpacked it and copied the contents to the `app/code/community/Nosto` directory, you've probably forgotten to update the `lib/Nosto` directory. 

Nosto places files under both the `app` the `lib` directory and you must remember to update all Nosto files in both directories.

We recommend you remove the `app/code/community/Nosto` and the `lib/Nosto` directory and install again.


## Why do I get errors such as `Base table or view not found: 1146 Table 'nosto_tagging_customer' doesn't exist`?

If you did an upgrade or installed Nosto by simply unpacking the files into the Magento directory, then it is possible that Magento did not run the upgrade scripts that create the necessary database tables and views.

We recommend you remove the `app/code/community/Nosto` and the `lib/Nosto` directory and install again using a command-line installer or Magento Connect Manager.

## Why is incomplete / cancelled order attributed to Nosto

Nosto's Magento extension send information about an order to Nosto via tagging and via API. Order is sent over the API to Nosto whenever an order (`Mage_Sales_Model_Order`) is saved in Magento. The definition of the observer can be [from here](https://github.com/Nosto/nosto-magento/blob/master/app/code/community/Nosto/Tagging/etc/config.xml#L126-L134). This means that order data gets sent to Nosto when the order is created or updated regardless of the status of the order. Nosto has pre-defined set of order statuses that are automatically ignored (cancelled, rejected, etc.). If the initial order status or order updates sent within 30 minutes of the purchase don't have status that is considered ignored the orders will be treated as valid in Nosto.

If a merchant provides a possibility for offline payments (cash on delivery, invoice, cheque, etc.) or 3rd party payment gateways that do not redirect back to the store Nosto has no way of knowing if the payment has been fulfilled or not. These orders are also considered as valid unless they are cancelled within that 30 minutes window.

More info about the attribution can be found from [here (Nosto's payment terms)](http://www.nosto.com/payment-terms/)