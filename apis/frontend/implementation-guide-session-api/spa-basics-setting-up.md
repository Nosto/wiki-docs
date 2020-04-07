# Setting up

## Setting up your account

You must use a valid domain for your website. If you are creating a test account and running your store locally, you must use valid TLD as localhost is not supported.

We do not recommend using a `.dev` or a `.test` TLD that is aliased to localhost. Both `.dev` and `.test` are reserved by Google and IANA for testing purposes. You will need to edit your operating-system dependent hosts file to add an alias for the domain you are using.

## Setting up the catalog sync

While Nosto crawls sites to replicate the product catalog, it is unable to do so on SPAs. In order to synchronise your product catalog with Nosto, you'll need to [leverage the product API](../../rest/products/updating-products-using-the-products-api.md) to keep the Nosto catalog in sync.

**Note:** This step must be completed prior to proceeding with the implementation. Without this step, it will be troublesome to preview and debug the recommendations.

## Including the script

To start tracking visits and content the Nosto script needs to be active on all pages within the store. Replace the account-id in the snippet below with your own account-id and place the code within the `<head>` section of your DOM. You can find your account-id from the admin.

The JS comprises of three parts - the first is the default Nosto snippet, the second is the "stub" \(which allows API usage prior to the script being loaded\), and last is the "autoload" configuration \(which prevents Nosto from automatically initiating once loaded.\)

```markup
<script src="//connect.nosto.com/include/$accountID" async></script>
<script type="text/javascript">
  (() => {window.nostojs=window.nostojs||(cb => {(window.nostojs.q=window.nostojs.q||[]).push(cb);});})();
</script>
<script type="text/javascript">
  nostojs(api => api.setAutoLoad(false));
</script>
```

**Note:** The script and the snippet should be added as high up in the `<head>` portion of the page so the connection is initialised as soon as possible. As the script is flagged `async`, the page load isn’t delayed.

**Note:** This needs to exist on every page.

