This endpoint is used for initiating a personal-data takeout request. All requests to this endpoint are asynchronous and simply enqueue the email address for takeout. It may take up to 24 hours for the takeout process to complete. If the email address is found, a notification email (informing you about the takeout request) will be sent once the process has completed.

#### Token

This endpoint requires an Email token.

### Usage

```shell
curl -v -X POST https://api.nosto.com/v1/customers/takeout/john.doe@nosto.com \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw
```