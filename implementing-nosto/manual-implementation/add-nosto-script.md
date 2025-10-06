# Adding the Nosto Script

To start tracking visits and content the Nosto script needs to be active on all pages within the store where the user might navigate. Replace `$accountID` from the code below with your own account ID and place the code within the `<head>` section of your sites HTML content. You can find your stores account IDs from the account list within the Nosto admin.

```markup
<script type="text/javascript">
    (function(){var name="nostojs";window[name]=window[name]||function(cb){(window[name].q=window[name].q||[]).push(cb);};})();
</script>
<script src="//connect.nosto.com/include/$accountID" async></script>
```

**Note:** The script and the snippet should be added as high up in the `<head>` portion of the page so the connection is initialised as soon as possible. As the script is flagged `async`, the page load isn’t delayed.

**Note:** This needs to exist on every page.

Alternatively to the script injection Nosto can also be used as a library dependency in your Javascript application via the following utility library [Nosto Js](https://github.com/Nosto/nosto-js)

## Troubleshooting Nosto script:

Once included on all pages, you can review if the site is transmitting data using the Nosto Debug Toolbar. If the debug toolbar executes and shows up on the page Nosto can track visits on the page. You can further verify your session in the Nosto admin by using the live feed under [https://my.nosto.com/admin/$accountID/liveFeed](https://my.nosto.com/admin/$accountID/liveFeed)

![Nosto debug toolbar](https://nosto-campaign-assets.s3.amazonaws.com/images/nosto-embed-script-debug.png)

