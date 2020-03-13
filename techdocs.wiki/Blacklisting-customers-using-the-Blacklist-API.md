This endpoint is used for blacklisting email addresses from receiving Nosto's triggered emails such as cart-abandonment emails, order-follow emails, and we-miss-you emails.

Having the corresponding email address in our system prior to blacklisting or unblacklisting is not a prerequisite. If the specified email record does not exist in our system, it will be created.

#### Token

This endpoint requires an Email token.

#### Blacklisting an email

You can specify multiple email addresses separated by a newline.

```shell
curl -v -X POST https://api.nosto.com/v1/email?op=add \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw -d '
john.doe@nosto.com
jane.doe@nosto.com'
```

#### Unblacklisting an email

You can specify multiple email addresses separated by a newline.

```shell
curl -v -X POST https://api.nosto.com/v1/email?op=remove \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw -d '
john.doe@nosto.com
jane.doe@nosto.com'
```