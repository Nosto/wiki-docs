# Marketing permission and GDPR compatibility
Marketing permission or newsletter constent from customers are recorded at Nosto either during order confirmation (Conversion Tracking) or through customerUpdate webhook (Customer Feed). In case of order confirmation, the consent is received through the `marketingPermission` field in `OrderCustomer` bean and though `acceptsMarketing` field in `ShopifyCustomer` bean in case of webhook. These fields tells Nosto whether the customer has given their [GDPR](https://www.eugdpr.org/) compliant consent for receiving marketing emails.

## Consent - Order confirmation
When a customer is placing an order for any product, the checkout page will collect the customer's consent for receiving the marketing emails as shown below,

![Consent during order confirmation](https://user-images.githubusercontent.com/82023195/136540098-3719ca4f-77e6-47d1-8dfa-525010885064.png)

When the customer selects `Email me with news and offers` option and confims the order, the consent along with order information will be sent to Nosto through `/order/track` API in the following format,

```json
{
    "info": {
        "order_number":"4321633632470",
        "type":"order",
        "email":"manikandan.ravikumar@nosto.com",
        "first_name":"Mani",
        "last_name":"Nosto", 
        "newsletter": "true"
    },
    "items": [{
        "product_id":"7190446571734",
        "sku_id":"41479723778262",
        "name":"Cool Kicks",
        "quantity":1,
        "unit_price":123.12,
        "price_currency_code":"EUR"
    }]
}
```

## Consent - Customer update
A merchant will be able to update customer's information or their marketing constent from store management (Admin) page. 

![Update Marketing Consent](https://user-images.githubusercontent.com/82023195/136540161-057d6c72-fa25-4785-aa56-19a48a016200.png)

During this process, shopify will share updated customer information through a registered webhook `https://webhooks.nosto.com/hub/shopify/customerFeed` in the following format,

```json
{
    "id": "12345",
    "email":"manikandan.ravikumar@nosto.com",
    "first_name":"Mani",
    "last_name":"Nosto", 
    "accepts_marketing": true
}
```

### Note: 
1. If your store has a custom process for gathering marketing permissions or the default logic explained above doesn't fit for your store, you can override Nosto's extension to amend the behaviour. Please read more detailed instructions in our guide on [Import customer data](https://help.nosto.com/en/articles/2884483-how-to-import-customer-data-to-nosto-via-api)
2. In order manually override marketing information for a customer email, follow the steps here [Manually Override Consent](https://docs.nosto.com/techdocs/apis/rest/customers/toggling-email-opt-in-using-the-consent-api)