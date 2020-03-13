This endpoint is used for toggling the marketing permission for an email. The marketing permission for an email is normally gathered [via the customer tagging](Adding-the-customer-information).

This endpoint is only intended for use when the consent needs to be programmatically managed and should be a considered an advanced use case.

#### Token

This endpoint requires an Email token.

### Usage

#### Granting consent

```shell
curl -v -X POST https://api.nosto.com/v1/customers/consent/john.doe@nosto.com/true \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw
```

#### Revoking consent

```shell
curl -v -X POST https://api.nosto.com/v1/customers/consent/john.doe@nosto.com/false \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw
```