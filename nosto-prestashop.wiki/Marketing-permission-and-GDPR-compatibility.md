As of version [3.3.0](https://github.com/Nosto/nosto-prestashop/releases/tag/3.3.0) Nosto's extension will be automatically adding the `marketing_permission` field to the customer tagging and to the conversion tracking i.e. the order-confirmation page. The marketing permission field tells Nosto whether the customer has given [GDPR](https://www.eugdpr.org/) compliant consent of receiving marketing via email or not.

![3.3.0](https://img.shields.io/badge/nosto-3.3.0-green.svg)

By default Nosto's extension uses Prestashop's newsletter subscription as a source of information for populating this field. In other words, if a customer is also a newsletter subscriber `marketing_permission` field will be set to `true` and Nosto considers that the customer has given GDPR compliant marketing permission consent. If a customer is not a newsletter subscriber `marketing_permission` field will be left empty and Nosto considers that the customer has not given GDPR compliant marketing permission.

[[/images/createaccount.png|ALT create_new_customer_account]]

If your store has a custom process for gathering marketing permissions or the default logic explained above doesn't fit for your store, you can override Nosto's extension to amend the behaviour. Please read more detailed instructions in our guide on [overriding the customer data](Overriding-Customer-Data).