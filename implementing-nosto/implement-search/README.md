# Implement Search & Categories

Nosto Search uses product and user behavior data from the Nosto Platform, so if you are implementing Nosto Search, first of all, you need to [implement Nosto Platform to your website](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website).

Your search engine will be ready after the Nosto representative enables the Search module for your account. Then you will need to integrate Nosto Search to the website.

## Implementation methods

### Search Templates Starter

For developers who prefer a modern, local development workflow, the Search Templates Starter provides a complete Preact-based project. This approach offers full source code control with Git, a local development environment with hot reloading, and a component library with pre-built, customizable search components. It's ideal for teams that want maximum flexibility and integration with their existing development practices.

{% content-ref url="using-search-templates-starter/" %}
[using-search-templates-starter](using-search-templates-starter/)
{% endcontent-ref %}

### Search Templates

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

|                                                | Search Templates Starter | Search Templates | API       | JavaScript Library |
| ---------------------------------------------- | ------------------------ | ---------------- | --------- | ------------------ |
| Can be implemented by Nosto team               | Yes                      | Yes              | No        | No                 |
| Expected time to launch live                   | 2-4 weeks\*              | 1-3 weeks\*      | 4-8 weeks | 3-6 weeks          |
| Headless compatible                            | Yes                      | Yes              | Yes       | Yes                |
| Fully customizable frontend                    | Yes                      | Yes              | Yes       | Yes                |
| Customized and managed only in Nosto dashboard | No                       | Yes              | No        | No                 |
| Suitable for complex use cases                 | Yes                      | Sometimes        | Yes       | Yes                |
| Merchandising rules applied automatically\*\*  | Yes                      | Yes              | Yes       | Yes                |
| Analytics                                      | Yes                      | Yes              | Yes       | Yes                |
| Segmentation                                   | Yes                      | Yes              | Yes       | Yes                |
| Individual personalization (affinities)        | Yes                      | Yes              | Yes       | Yes                |
| A/B testing                                    | Yes                      | Yes              | Yes       | Yes                |
| SPA suitable                                   | Yes                      | Limited\*\*\*    | Yes       | Yes                |

{% hint style="info" %}
\* This estimation is based on the merchant's team building the templates. When Nosto's frontend team builds templates via the Code Editor, this can take longer due to overall bandwidth from the team.

\*\* Matching merchandising rules are applied automatically based on requested search queries, categories, and segments, without the need to request them in API requests.

\*\*\* Using search templates with SPAs comes with challenges related to routing and dynamic content injection that tend to be solvable, but are more technically involved. We highly recommend using the JavaScript library or API instead.
{% endhint %}

### Mixing integration types ("hybrid integration")

With appropriate care, it's possible to mix and match different integration types.
The most commonly observed combinations are:

* Use JavaScript library for tracking and API for retrieving search/category results.
* Use search templates (starter) for autocomplete, and API for search/category results.

When choosing such an approach, it's important that all components involved use the same session parameters throughout to ensure that all interactions are connected to the same visitor.
This is easiest to achieve by managing the session in the frontend using the JavaScript library (included in search templates (starter)), and passing session parameters (including active segments and personalization affinities) to other components connected to Nosto (e.g., a backend interacting with Nosto search via API), so these can include the session parameters in API requests.

To avoid pitfalls and ensure correct attribution of sales to search, tracking search impressions and clicks should be done with the same integration types.

Please refer to documentation for retrieving session parameters via [JavaScript library](./search#session-parameters) or [API](./implement-search-using-api/analytics-ab-testing.md#query-session) for more details.

### Search Templates vs. Search Templates Starter

| Feature                     | Search Templates                                        | Search Templates Starter                       |
| --------------------------- | ------------------------------------------------------- | ---------------------------------------------- |
| **Development Environment** | In-browser code editor in the Nosto Admin UI            | Local development with your preferred IDE      |
| **Version Control**         | Managed within the Nosto platform                       | Full source code control with Git              |
| **Workflow**                | Edit and preview directly in the browser                | Local development with `nosto-cli` for uploads |
| **Best For**                | Quick setup and users comfortable with an online editor | Developers wanting a modern, local workflow    |

If you are a developer who prefers a modern, local development workflow with full version control (Git), we strongly recommend using the **Search Templates Starter**. It offers the most flexibility and integrates seamlessly with professional development practices.

If you are looking for a faster, more straightforward setup and are comfortable using an in-browser code editor, **Search Templates** are a great alternative.

For use cases that require deep backend integration, or if you're building for a non-web platform (e.g., native mobile apps), the **API** or **JavaScript Library** integrations provide the necessary control and access to the raw search data.
