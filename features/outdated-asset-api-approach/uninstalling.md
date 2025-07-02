# Uninstalling

If you wish to remove Nosto from your store, please first read our help article on [terminating service](https://help.nosto.com/en/articles/586729-how-to-terminate-service).

The Nosto app applies changes to the theme when it is installed. Uninstalling the app does not automatically remove these changes. Below are instructions on how to remove the theme changes.

### Remove Theme Changes Using the Nosto App <a id="remove-theme-changes-using-the-nosto-app"></a>

This is the most straightforward way of removing theme changes. If you have already uninstalled the Nosto app, you can re-install the app, follow these steps and then uninstall again.

Navigate to the Nosto app in the Apps section of Shopify admin.

![](https://gblobscdn.gitbook.com/assets%2F-M4IGmkA5HKprITT1GHf%2F-MRxdrLQ5LO_4RqRSYty%2F-MRxo8AxhInc8DI6FFSS%2Fimage.png?alt=media&token=517e7683-a035-42db-8722-1a9140c77663)

Go to the Settings tab and the scroll down to the bottom of the page. There you will see buttons to remove Nosto from a theme. You can simply click the **Remove Nosto** button to remove changes from all themes.

Please note that this operation is performed in the background, so may take a minute or so to complete.

![](https://gblobscdn.gitbook.com/assets%2F-M4IGmkA5HKprITT1GHf%2F-MRxdrLQ5LO_4RqRSYty%2F-MRxoUfZZr2CF7mBTATZ%2Fimage.png?alt=media&token=4acb0ca1-66a4-42cd-8166-65a72dd3fb8d)

### Manually Remove Theme Changes <a id="manually-remove-theme-changes"></a>

The Nosto app adds the following snippets to the theme, which can be removed:

* `snippets/nosto-element.liquid`
* `snippets/nosto-script.liquid`
* `snippets/nosto-tagging.liquid`

Remove any references to Nosto from the following files:

* `layout/theme.liquid`
* `templates/index.liquid`
* `templates/product.liquid`
* `snippets/product.liquid`
* `templates/search.liquid`
* `templates/404.liquid`
* `templates/collection.liquid`
* `templates/cart.liquid`

You may need to remove references to Nosto from other theme files if you have made any custom modifications.

