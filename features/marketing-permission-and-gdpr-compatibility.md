# Marketing permission and GDPR compatibility

![3.6.1](https://img.shields.io/badge/nosto-3.6.1-green.svg)

As of version [3.6.1](https://github.com/Nosto/nosto-magento/releases/tag/3.6.1) Nosto's Magento extension will be automatically adding `marketing_permission` field to the customer tagging and to the buyer \("customer"\) object in order confirmation. The marketing permission field tells Nosto whether the customer has given [GDPR](https://www.eugdpr.org/) compliant consent of receiving marketing via email or not.

By default Nosto's Magento extension uses Magento's newsletter subscription as a source of information for populating this field. In other words if a customer is also a newsletter subscriber `marketing_permission` field will be set to `true` and Nosto considers that the customer has given GDPR compliant marketing permission consent. If a customer is not a newsletter subscriber `marketing_permission` field will be set to `false` and Nosto considers that the customer has not given GDPR compliant marketing permission.

![create\_new\_customer\_account\_m](https://user-images.githubusercontent.com/2778820/38495735-5bbe2d76-3c03-11e8-8123-46d7263cef76.png)

If your store has a custom process for gathering marketing permissions or the default logic explained above doesn't fit for your store, you can override Nosto's extension to amend the behaviour. Please read more detailed instructions in our guide on [overriding the customer data](../guides/overriding-or-extending-functionalities/overriding-customer-data.md).

