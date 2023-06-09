# Implementing Search page

## Configuration

To create a search application, call the `init` function with your configuration. This will create a new Preact application that renders on the specified `contentCssSelector`. It will also bind to the input element identified by the provided `inputCssSelector` and execute a search upon form submission.&#x20;

<pre class="language-javascript" data-title="index.js"><code class="lang-javascript"><strong>import { init } from '@nosto/preact'
</strong>
import serpComponent from './serp'

init({
    ...window.nostoTemplatesConfig,
    serpComponent,
    inputCssSelector: '#search',
    contentCssSelector: '#content',
    serpPath: '/search',
    serpUrlMapping: {
        query: 'q',
    },
    serpQuery: {
        name: 'serp',
        products: {
            size: 20,
            from: 0,
        },
    },
})
</code></pre>

### Search page path

When `serpPath` parameter is specified, the application will **redirect to the specified search** page after a search is conducted. Otherwise, the search will be **rendered** **on the same page** without changing the URL path.

### Serp component

Search results page component should render full search page using provided app state. Minimal example could look like:

{% code title="serp/index.js" %}
```jsx
import { useAppState, SerpElement } from '@nosto/preact'

export default () => {
    const { response: { products }, loading } = useAppState()

    return (
        <div>
            {loading && <div>Loading...</div>}
            {products.total ? <div>
                {products.hits.map(hit => <SerpElement as="a" hit={hit}>
                    {hit.name}
                    {hit.price} 
                </SerpElement>)}
            </div> : <div>
                No results were found
            </div>}
        </div>
    )
}
```
{% endcode %}

## Features

### Faceted navigation

#### Stats facet

The stats facet returns the minimum and maximum values of numerical fields. It is useful for creating sliders, such as a price slider.

{% code title="serp/facets/stats.js" %}
```jsx
import { RangeSlider } from '@nosto/preact'

export default ({ facet }) => {
    return (
        <RangeSlider id={facet.id} />
    )
}
```
{% endcode %}

### Pagination

Pagination can be created using `usePagination` helper and `updateSearch` action:

{% code title="serp/pagination.jsx" %}
```jsx
import { usePagination, useActions } from '@nosto/preact'

export default () => {
    const pagination = usePagination({
        width: 5
    })
    const { updateSearch } = useActions()
    
    const createCallback = (from) => () => {
        updateSearch({
            products: {
                from,
            },
        })
        scrollTo(0, 0)
    }

    return (
        <ul>
            {pagination.prev && <li>
                <a
                    href="javascript:void(0)"
                    onClick={createCallback(pagination.prev.size)}
                >
                    prev
                </a>
            </li>}
            {pagination.first && <li>
                <a
                    href="javascript:void(0)"
                    onClick={createCallback(pagination.first.from)}
                >
                    {pagination.first.page}
                </a>
            </li>}
            {pagination.first && <li>...</li>}
            {pagination.pages.map((page) => <li class={page.current ? "active" : ""}>
                <a
                    href="javascript:void(0)"
                    onClick={createCallback(page.from)}
                >
                    {page.page}
                </a>
            </li>)}
            {pagination.last && <li>...</li>}
            {pagination.last && <li>
                <a
                    href="javascript:void(0)"
                    onClick={createCallback(pagination.last.from)}
                >
                    {pagination.last.page}
                </a>
            </li>}
            {pagination.next && <li>
                <a
                    href="javascript:void(0)"
                    onClick={createCallback(pagination.next.offset)}
                >
                    <span aria-hidden="true">
                        <i class="ns-icon ns-icon-arrow"></i>
                    </span>
                </a>
            </li>}
        </ul>
    )
}
```
{% endcode %}

### Analytics

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

## Search engine configuration <a href="#selecting-fields" id="selecting-fields"></a>

Nosto Search engine is relevant out of the box and search API can be used without any initial setup. Nosto Dashboard can be used to further tune search engine configuration:

* [Searchable Fields](https://help.nosto.com/en/articles/7161528-search-engine-s-logic-and-searchable-fields) - manage which fields are used for search and their priorities,
* [Facets](https://help.nosto.com/en/articles/7169091-setting-up-facets) - create facets (filtering options) for search results page,
* [Ranking and Personalization](https://help.nosto.com/en/articles/7168969-merchandising-search-personalization-guide) Ranking and Personalization - manage how results are ranked,
* Synonyms, Redirects, and other search features are also managed through Nosto Dashboard ([my.nosto.com](https://my.nosto.com/)).
