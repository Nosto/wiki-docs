# Implementing Category pages

Category pages can be rendered using search templates over existing category pages.

{% hint style="info" %}
It's recommended to disable or hide (for instance, using CSS) the original category page products to avoid displaying them while new results are loading. This prevents any possible confusion for users as the updated results are populated.
{% endhint %}

## Configuration

To render category page provide additional configuration parameters to `init` function on `index.js` entrypoint file.

{% code title="index.js" %}
```javascript
import { init } from '@nosto/preact'
import categoryComponent from './category'

init({
    ...window.nostoTemplatesConfig,
    inputCssSelector: '#search',
    contentCssSelector: '#content', // or categoryCssSelector
    categoryComponent: categoryComponent,
    categoryQuery: () => {
        // extract category ID or category name from current page
        const categryPath = document.querySelector('.category_name')
        return {
            name: 'serp',
            products: {
                categoryPath: categryPath?.textContent,
                // or categoryId: categryPath?.textContent
                size: 30,
                from: 0
            }
        }
    },
    isCategoryPage: () => {
        // Detect if category page should be rendered here
        return location.pathname.startsWith('/collections/')
    }
})
```
{% endcode %}

### Detect search page

The `isCategoryPage` function should detect whether the current page is a category page. Its result should not be cached, as it needs to dynamically detect the page type since it can change during the app's execution. For example, if the user navigates from a category page to a search page, the function should reflect that change in page type.

### Category query

The `categoryQuery` should generate a category page query based on the current page. It must return either `categoryPath` (identical to the `categories` field filter) or `categoryId` to select the current category.&#x20;

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

Search automatically tracks to Google Analytics & Nosto Analytics when using `<`SerpElement `/>` component.&#x20;

```jsx
export default ({ product }) => {
    return (
        <SerpElement as="a" hit={product}>
            {product.name}
        </SerpElement>
    )
}
```