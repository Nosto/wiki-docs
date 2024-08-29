# Implementing Category pages

Category pages can be rendered using search templates over existing category pages.

## Configuration

To render the category page, provide additional configuration parameters to the `init` function in the `index.js` entry point file. Default configurations for `categoryQuery` and `isCategoryPage` are already provided. Custom configuration is necessary only if the default settings are not suitable for your application.

The default `isCategoryPage` function checks for the presence of an element in the DOM and determines if the page should be considered a category page based on its content.

{% code title="index.js" %}
```javascript
import { init } from '@nosto/preact'
import categoryComponent from './category'

init({
    ...window.nostoTemplatesConfig,
    inputCssSelector: '#search',
    contentCssSelector: '#content', // or categoryCssSelector
    categoryComponent: categoryComponent,
    categoryCssSelector: '#MainContent',

    // // There is a default implementation for categoryQuery,
    // // so use this custom configuration only if you need to customize it:
    // //
    // categoryQuery: () => {
    //     return {
    //         name: 'serp',
    //         products: {
    //             categoryId: this.categoryId(),
    //             categoryPath: this.categoryPath(),
    //             size: defaultConfig.serpSize,
    //             from: 0,
    //         }
    //     };
    // },
    
    // // If you want to integrate only Categories without Search or Autocomplete,
    // // ensure that the following entries are removed or commented out:
    // //
    // serpComponent,
    // historyComponent,
    // autocompleteComponent,
})
```
{% endcode %}

### Handling native results

It's recommended to hide the original category page products to avoid displaying them while Nosto is initializing. This prevents any possible confusion for users and avoids visual flickering of the page.

The best approach is to add `ns-content-hidden` classname to the same element you are targeting with `contentCssSelector` or `categoryCssSelector`. This classname will be stripped away by Nosto automatically as soon as the script is initialized.

In addition, you should define CSS to hide the target element:

{% code title="css" %}
```css
.ns-content-hidden {
    display: none;
}
```
{% endcode %}

{% hint style="info" %}
You are free to change the exact styles use as long as they hide the content from view and the class name matches the one Nosto is looking for.
{% endhint %}

It is not recommended to completely block or remove native results from the page as Nosto will attempt to display the original category page products in case Nosto service is unavailable or can't be reached. In addition, the original products are made available for the SEO crawlers, improving your page's ranking in the search engines.

### Detect search page

The `isCategoryPage` function should detect whether the current page is a category page. Its result should not be cached, as it needs to dynamically detect the page type since it can change during the app's execution. For example, if the user navigates from a category page to a search page, the function should reflect that change in page type.

### Category query

The `categoryQuery` should generate a category page query based on the current page. It must return either `categoryPath` (identical to the `categories` field filter) or `categoryId` to select the current category. The default implementation extracts the `categoryId` and `categoryPath` fields from the DOM.

{% hint style="warning" %}
Search rules targeting specific categories are not supported when using `categoryPath` parameter, they supported only for `categoryId` parameter
{% endhint %}

### Category component

The category component should also be implemented. In most cases, it is the same component as the search page component, except that the search query should not be displayed.

{% code title="category/index.jsx" %}
```jsx
import { useAppState } from '@nosto/preact'

export default () => {
    const { response: { products }, loading } = useAppState()

    return (
        <div>
            {loading && <div>Loading...</div>}
            {products.total ? <div>
                {products.hits.map(hit => <div>
                    {hit.name}
                    {hit.price} 
                </div>)}
            </div> : <div>
                Category is empty
            </div>}
        </div>
    )
}
```
{% endcode %}

### Other features & implementation

The category page shares a lot of similarities with the search page, so please refer to the search page documentation:

{% content-ref url="../implement-search-using-code-editor/implementing-search-page.md" %}
[implementing-search-page.md](../implement-search-using-code-editor/implementing-search-page.md)
{% endcontent-ref %}

## Analytics

Search automatically tracks to Google Analytics & Nosto Analytics when using the `SerpElement` component.

```jsx
export default ({ product }) => {
    return (
        <SerpElement as="a" hit={product}>
            {product.name}
        </SerpElement>
    )
}
```
