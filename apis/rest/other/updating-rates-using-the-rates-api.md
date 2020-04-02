# Updating Rates

When using multi-currency, this endpoint is used to update the exchange rates for your account. When new rates are sent via this endpoint, the changes will reflect instantly on your store as the product prices are multiplied with the provided rates in real-time.

**Note:** This endpoint should only be used when using multi-currency. Please refer to our multi-currency guide prior to using this endpoint. Incorrect usage of these endpoints will result in a total outage of your personalisation setup.

### Token

This endpoint requires a Rates token.

## Usage

```text
curl -v -X POST https://api.nosto.com/exchangerates \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw \
-H "Content-Type: application/json" -d '
{
  "rates":{
    "GBP":{
      "rate":0.77,
      "price_currency_code":"GBP"
    },
    "EUR":{
      "rate":0.91,
      "price_currency_code":"EUR"
    }
  },
  "valid_until":"2015-02-27T12:00:00Z"
}'
```

## How can I get an API token?

You can request an API token \(API\_PRODUCTS\) by getting in touch with our support personnel. Once the token has been granted, you will be able to find it listed in the [authentication tokens section in the admin.](https://help.nosto.com/settings-and-troubleshooting-faq/settings-authentication-tokens)

## How often should I send exchange-rates?

You can send exchange rates as often as you like but at the bare minimum, the exchange rates should be sent when they are changed.

