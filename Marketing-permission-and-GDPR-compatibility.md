As of version [2.0.0](https://github.com/Nosto/nosto-shopware/releases/tag/2.0.0) Nosto's extension will be automatically adding the `marketing_permission` field to the customer tagging and to the buyer ("customer") object in the conversion tracking i.e. the order-confirmation page. The marketing permission field tells Nosto whether the customer has given [GDPR](https://www.eugdpr.org/) compliant consent of receiving marketing via email or not.

![2.0.0](https://img.shields.io/badge/nosto-2.0.0-green.svg)

By default Nosto's extension uses Shopware's newsletter subscription as a source of information for populating this field. In other words, if a customer is also a newsletter subscriber `marketing_permission` field will be set to `true` and Nosto considers that the customer has given GDPR compliant marketing permission consent. If a customer is not a newsletter subscriber `marketing_permission` field will be set to `false` and Nosto considers that the customer has not given GDPR compliant marketing permission.

![customer_account___english](https://user-images.githubusercontent.com/2778820/38422547-d0a19c38-39b3-11e8-9acc-c832fd0be302.png)

If your store has a custom process for gathering marketing permissions or the default logic explained above doesn't fit for your store, you can override Nosto's extension to amend the behaviour. Please read more detailed instructions in our guide on [overriding the customer data](Overriding-Customer-Data.md).

## Disabling customer tagging completely
See [Nosto settings](Configuring.md#customer-tagging)