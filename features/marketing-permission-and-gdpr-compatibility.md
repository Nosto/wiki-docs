# Marketing permission and GDPR compatibility

As of version [2.8.0](https://github.com/Nosto/nosto-magento2/releases/tag/2.8.0) Nosto's Magento 2 extension will be automatically adding the `marketing_permission` field to the customer tagging and to the buyer \("customer"\) object in the conversion tracking i.e. the order-confirmation page. The marketing permission field tells Nosto whether the customer has given [GDPR](https://www.eugdpr.org/) compliant consent of receiving marketing via email or not.

![2.8.0](https://img.shields.io/badge/nosto-2.8.0-green.svg)

By default Nosto's Magento 2 extension uses Magento's newsletter subscription as a source of information for populating this field. In other words, if a customer is also a newsletter subscriber `marketing_permission` field will be set to `true` and Nosto considers that the customer has given GDPR compliant marketing permission consent. If a customer is not a newsletter subscriber `marketing_permission` field will be set to `false` and Nosto considers that the customer has not given GDPR compliant marketing permission.

**Note:** f you are using Magento &lt; 2.1.10 the marketing permission info is not sent to Nosto in real time when the user updates the newsletter subscription field. This is due to a [Magento bug](https://github.com/magento/magento2/issues/6818) that was fixed in 2.1.10.

![create\_new\_customer\_account](https://user-images.githubusercontent.com/15191701/38088088-49ef30fc-3363-11e8-9287-d5b4911e23b0.png)

If your store has a custom process for gathering marketing permissions or the default logic explained above doesn't fit for your store, you can override Nosto's extension to amend the behaviour. Please read more detailed instructions in our guide on [overriding the customer data](https://github.com/supercid/wiki-docs/tree/bfcad31e54e9c48d9a2ca1df7b28ae4691497212/Overriding-Customer-Data/README.md).

