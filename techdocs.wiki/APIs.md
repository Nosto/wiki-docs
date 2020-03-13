Nosto offers a multitude of APIs for different use cases. The APIs are not entirely RESTful but provide lightweight endpoints that expose similar usability.

All the APIs reside at `https://api.nosto.com` and must be accessed over HTTPS.

**Note:** If you happen to call the interface via HTTP using a valid API key, that API key will be invalidated immediately and a notice of the token revocation will be sent to the account owner.

## Authentication

Authenticating with the API is done by using ["Basic" authentication](https://tools.ietf.org/html/rfc7617). You authenticate by using your API key as the password and the username is left empty.

You can see your API keys in the Nosto Backend under account settings. API key is always tied to a single store in your account. 

**Note**: Keep your API key secret and delete it immediately if you think someone untrusted might have had access to it.

| HTTP Header   | Value                    |
| ------------- | ------------------------ |
| Authorization | `Basic :NOSTO_API_TOKEN` |

##### Requesting access

To get access to our APIs, please log in to your Nosto account at [https://my.nosto.com](https://my.nosto.com/) and contact support via chat. When you request API access, please provide following information:

- What is the API in question?
- What is the purpose for the API use?
- What is the volume of requests?
- What is the request distribution, on-demand or periodic?

##### Token types

You can get token values from [authentication tokens](https://help.nosto.com/en/articles/613616-settings-authentication-tokens) page under your Nosto Account. Each set of endpoints are secured using different token types.

| Token type      | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| API_REC         | A deprecated token for using the legacy recommendations API  |
| API_PRODUCTS    | A token for accessing the Products API                       |
| API_EMAIL       | A token for managing customers using the Blacklist and GDPR APIs |
| API_APPS        | A token for accessing the new GraphQL APIs                   |
| API_OMNICHANNEL | A token for accessing the new Omni-channel API               |
| API_RATES       | A token for updating rates using the Exchange Rates API      |
| API_SETTINGS    | A token for configuring your account using the Settings API  |

## Rate Limits

Nosto does not rate-limit the API usage but follows a fair-use policy. Nosto reserves the right to revoke API access for any abusive API usage patterns.