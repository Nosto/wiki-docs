The module adds tagging on multiple pages across the shop which are used by the Nosto script as a context when fetching recommendations.

Sometimes it so happens that some parts of the tagging are missing on the page. For example, you'll be able to see the debug toolbar but there would be no markup. The root cause of this scenario is that one of more hooks are missing from your theme.

In order to rectify this, you'll need to place the missing hooks back into the corresponding template file of your theme. Below is a comprehensive list of all the hooks that are used for injecting semantic metadata into your theme.

| Page     | Description                      | Hook                       |
|----------|----------------------------------|----------------------------|
| All      | Customer Tagging                 | [`HOOK_TOP`](https://github.com/PrestaShop/PrestaShop/blob/1.6.1.9/themes/default-bootstrap/header.tpl#L110)                 |
| All      | Cart Tagging                     | [`HOOK_TOP`](https://github.com/PrestaShop/PrestaShop/blob/1.6.1.9/themes/default-bootstrap/header.tpl#L110)                 |
| All      | Current Category / Brand Tagging | [`HOOK_TOP`](https://github.com/PrestaShop/PrestaShop/blob/1.6.1.9/themes/default-bootstrap/header.tpl#L110)                 |
| All      | Search Term Tagging              | [`HOOK_TOP`](https://github.com/PrestaShop/PrestaShop/blob/1.6.1.9/themes/default-bootstrap/header.tpl#L110)                 |
| Product  | Product Page Recommandation Placeholders                  | [`HOOK_PRODUCT_FOOTER`](https://github.com/PrestaShop/PrestaShop/blob/1.6.1.9/themes/default-bootstrap/product.tpl#L563)      |
| Order    | Order Confirmation Page Recommandation Placeholders                   | [`HOOK_ORDER_CONFIRMATION`](https://github.com/PrestaShop/PrestaShop/blob/1.6.1.9/themes/default-bootstrap/order-confirmation.tpl#L35)  |

In the event that one or more of the tagging blocks do not render into the markup, please ensure that your theme is not missing the default hook placeholders. Examples of what hook references look like in the default Prestashop templates can be viewed by clicking the links in the "Hook" section

More information on how the tagging works can be found on our [tagging guide](http://my.nosto.com/tagging).