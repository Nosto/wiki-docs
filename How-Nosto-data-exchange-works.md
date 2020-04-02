First its important to understand a few of the concepts before delving in to the documentation, to make sure that you as a integrator understand the different steps and tools required. 

* Nosto initializes by using a [snippet of Javascript](Add-Nosto-script.md) that starts the dialogue to your dedicated account. 
* Nosto depends on structured data that we extract from your site. At the bare minimum you need to go through all the steps listed under [Manual Implementation - Essentials](Manual-implementation.md).
* You can access your own account in the Nosto admin UI ([https://my.nosto.com/admin](my.nosto.com/admin)) where you can enable/configure/modify features. All templating and layouts are handled within your account.

You can read more about how are crawling mechanism works under [How does the Nosto crawler work?](Nosto-crawler.md) to find out how the front-end implementation methodology functions.

If you for some reason require to explicitly schedule our crawler with cronjobs or events you can do that with our Recrawl API: [Updating products using the Recrawl API](Updating-products-using-the-Recrawl-API.md) or if your page for some reason is not crawlable you can send us product details using our Products API: [Sending products manually with the Products API](Sending-products-manually-with-the-Products-API.md)  