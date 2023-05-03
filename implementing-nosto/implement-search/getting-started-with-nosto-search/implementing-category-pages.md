# Implementing Category pages

Category pages can be rendered using search templates over existing category pages to .

{% hint style="info" %}
It is best to have already-existing category pages and disable or hide current results rendering in category pages to achieve the best results.&#x20;
{% endhint %}

To render category page provide additional configuration parameters to `init` function on `index.js` entrypoint file:

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

Category component should also be implemented. In most cases is the same component as search page component, just instead of search query should not be shown.&#x20;
