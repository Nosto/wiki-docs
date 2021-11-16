# How Nosto modifies your theme

Upon installing the Nosto for Shopify app, choosing a theme to edit, Nosto amends the core theme file called `layout/theme.liquid` and amends it to include the Nosto tagging snippet. To better understand snippets and the Shopify theme structure, we recommend reading [Shopify's theme development guide](https://help.shopify.com/themes/development/templates#snippets).

## Amendments to include the Nosto tagging

Nosto amends the core theme file (`layout/theme.liquid`) to include the Nosto semantic markup snippet using the following include:

```
{% render 'nosto-tagging' %}
```

A corresponding snippet file named `nosto-tagging.liquid` is uploaded to the `snippets` directory of your theme. The snippet contains all the Liquid templates needed to render markup that gives Nosto insights into the page context.
