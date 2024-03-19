# Implement Search & Categories

Nosto Search uses product and user behavior data from the Nosto Platform, so if you are implementing Nosto Search, first of all, you need to [implement Nosto Platform to your website](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website).

Your search engine will be ready after the Nosto representative enables the Search module for your account. Then you will need to integrate Nosto Search to the website.

## Implementation methods

### Code Editor

By using a pre-built template that can be customized to fully match a website's design using built-in code editor directly in [https://my.nosto.com/](https://my.nosto.com/). In the code editor, you can fully customize the pre-built template's JavaScript, HTML, and CSS code. Changes to the template can be implemented either by client’s developers or Nosto team. Frontend integration uses Preact and JSX templates and renders search results page dynamically in website’s frontend, so no additional integration is needed to the backend. When using Nosto services, no development is needed from the client.

{% content-ref url="implement-search-using-code-editor/" %}
[implement-search-using-code-editor](implement-search-using-code-editor/)
{% endcontent-ref %}

### API

API integration - a robust Search GraphQL API allows to implement Nosto Search into any website or app and gives complete flexibility for developers to build frontend and backend features.

{% content-ref url="implement-search-using-api/" %}
[implement-search-using-api](implement-search-using-api/)
{% endcontent-ref %}

### JavaScript Library

For frontend integrations you can also use our JavaScript library. This library wraps the Search GraphQL API and provides its functionality in a programmatic way.

{% content-ref url="search/" %}
[search](search/)
{% endcontent-ref %}

## Compare implementations

|                                                | Code Editor | API       | JavaScript Library |
| ---------------------------------------------- | ----------- | --------- | ------------------ |
| Can be implemented by Nosto team               | Yes         | No        | No                 |
| Expected time to launch live                   | 1-3 weeks   | 4-8 weeks | 3-6 weeks          |
| Headless compatible                            | Yes         | Yes       | Yes                |
| Fully customizable frontend                    | Yes         | Yes       | Yes                |
| Customized and managed only in Nosto dashboard | Yes         | No        | No                 |
| Suitable for complex use cases                 | Sometimes   | Yes       | Yes                |

If you are looking for a fast launch without much effort we recommend going with the fully customizable pre-built templates. This type of integration does not support full API access but comes complete with an out-of-the-box search result page and autocomplete templates that can easily be customized to match most website designs and integrate even advanced custom functionality.

If however you need full control over the search frontend or require complex custom functionality we recommend to go with the API or JavaScript integrations. These integrations do not provide out-of-the-box templates but provide direct access to the Search API, allowing you use the data in whatever way is required for your use cases.

