# Implement Search

Nosto Search uses product and user behavior data from the Nosto Platform, so if you are implementing Nosto Search, first of all, you need to [implement Nosto Platform to your website](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website).

Your search engine will be ready after the Nosto representative enables the Search module for your account. Then you will need to integrate Nosto Search to the website.

## Implementation methods

### Code Editor

By using a pre-built template that can be customized to fully match a website's design using built-in code editor directly in [https://my.nosto.com/](https://my.nosto.com/). In the code editor, you can fully customize the pre-built template's JavaScript, HTML, and CSS code. Changes to the template can be implemented either by client’s developers or Nosto team. Frontend integration uses Preact and JSX templates and renders search results page dynamically in website’s frontend, so no additional integration is needed to the backend. When using Nosto services, no development is needed from the client.

{% content-ref url="getting-started-with-nosto-search/" %}
[getting-started-with-nosto-search](getting-started-with-nosto-search/)
{% endcontent-ref %}

### API

API integration - a robust Search GraphQL API allows to implement Nosto Search into any website or app and gives complete flexibility for developers to build frontend and backend features.

{% content-ref url="implement-search-using-api/" %}
[implement-search-using-api](implement-search-using-api/)
{% endcontent-ref %}

## Compare implementations

|                                                | Code Editor | API       |
| ---------------------------------------------- | ----------- | --------- |
| Can be implemented by Nosto team               | Yes         | No        |
| Expected time to launch live                   | 1-3 weeks   | 4-8 weeks |
| Headless compatible                            | Yes         | Yes       |
| Fully customizable frontend                    | Yes         | Yes       |
| Customized and managed only in Nosto dashboard | Yes         | No        |
| Suitable for complex use cases                 | Sometimes   | Yes       |

Looking at the comparison table, we recommend going with fully customizable pre-built templates if you are looking for a fast launch without much effort.

With both Frontend and API integration, you have complete control over the search frontend and can fully match the website's design. Frontend integration may be easier to launch quickly because it has almost working out of the box search page and autocomplete templates that can be easily customized to match website design. However, if the search page design is quite complex and requires custom logic, it may be worth considering using API integration as it may be difficult to implement complex logic using built-in code editor.



