# Setting up your account

You must use a valid domain for your website. If you are creating a test account and running your store locally, you must use valid TLD as localhost is not supported.

We recommend using a `.dev` or a `.test` TLD that is aliased to localhost. Both `.dev` and `.test` are reserved by Google and IANA for testing purposes. You will need to edit your operating-system dependent hosts file to add an alias for the domain you are using.

Nosto periodically crawls your website to keep the catalog data in sync and therefore if you run your webshop locally, Nosto will be unable to crawl your website. In order to overcome this, you will either need to:

1. make it publicly available using a service such as [Pagekite](https://pagekite.net/) or [Ngrok](https://ngrok.com/)
2. Use the [Products API](../../apis/rest/products/updating-products-using-the-products-api.md) to keep your catalog in sync

