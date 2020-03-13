This endpoint is used for redacting all personal data associated with an email. All requests to these endpoints are asynchronous and simply enqueue the email address for redaction. It may take up to 24 hours for the redaction process to complete. If the email address is found, a notification email (informing you about the redaction) will be sent once the process has completed.

#### Token

This endpoint requires an Email token.

### Usage

```shell
curl -v -X DELETE https://api.nosto.com/v1/customers/redact/john.doe@nosto.com \
--user :WI0j2oN7TgG42tlblX3yzOQ5xvCYc2oYj9eWg79lghVq8R0nKQXlVE9wvihBUFOw
```