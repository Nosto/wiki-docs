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

The search results page component should render a full search page using the provided app state. A minimal example might look like this:

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

The [stats facet](https://search.nosto.com/v1/graphql?ref=SearchStatsFacet) returns the minimum and maximum values of numerical fields from search results. This functionality is especially useful when creating interactive elements like sliders. For instance, a price slider can leverage these min-max values to define its range, providing users a simple way to filter products within their desired price range.

{% code title="serp/facets/stats.js" %}
```jsx
import { RangeSlider, useRangeSlider } from '@nosto/preact'

export default ({ facet }) => {
    const {
        min,
        max,
        range,
        updateRange
    } = useRangeSlider(facet.id)

    return  <div>
        <h2>{facet.name}</h2>
        <label>
            Min.
            <input type="number" value={range[0]} min={min} max={max} onChange={(e) => {
                const value = parseFloat(e.currentTarget.value) || undefined
                updateRange([value, range[1]])
            }}/>
        </label>
        <label>
            Max.
            <input type="number" value={range[1]} min={min} max={max} onChange={(e) => {
                const value = parseFloat(e.currentTarget.value) || undefined
                updateRange([range[0], value])
            }} />
        </label>
        <RangeSlider id={facet.id} />
    </div>
}
```
{% endcode %}

Utilize the useRangeSlider hook to generate useful context for rendering range inputs. Additionally, employ the component to generate the slider itself. These tools together facilitate the creation of dynamic and interactive range sliders for your application.

#### Terms facet

The [terms facet](https://search.nosto.com/v1/graphql?ref=SearchTermsFacet) returns field terms for all products found in the search. This feature analyzes the content of each product and extracts meaningful terms. These terms can then be used to filter or refine search results, providing users with a more accurate and targeted product search.

```jsx
import { useActions } from '@nosto/preact'

export default ({ facet }) => {
    const { toggleProductFilter } = useActions()

    return <div>
        <h2>{facet.name}</h2>
        <ul>
            {facet.data?.map((value) => <li>
                <label>
                    {value.value}
                    <input
                        type="checkbox"
                        checked={value.selected}
                        onChange={(e) => {
                            e.preventDefault()
                            toggleProductFilter(
                                facet.field,
                                value.value,
                                !value.selected
                            )
                        }}
                    />
                </label>
                ({value.count})
            </li>)}
        </ul>
    </div>
}
```

You can use the toggleProductFilter function to toggle any filter value. This function will either add the filter value if it's not already applied or remove it if it's currently active, thus providing an efficient way to manipulate product filters in your application.

### Pagination

Use the `usePagination` hook to generate useful context for rendering any desired pagination. Utilize the width parameter to adjust how many page options should be visible. Also, remember to scroll to the top on each page change to ensure a seamless navigation experience for users.

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

## Search engine configuration <a href="#selecting-fields" id="selecting-fields"></a>

Nosto Search engine is relevant out of the box and search API can be used without any initial setup. Nosto Dashboard can be used to further tune search engine configuration:

* [Searchable Fields](https://help.nosto.com/en/articles/7161528-search-engine-s-logic-and-searchable-fields) - manage which fields are used for search and their priorities,
* [Facets](https://help.nosto.com/en/articles/7169091-setting-up-facets) - create facets (filtering options) for search results page,
* [Ranking and Personalization](https://help.nosto.com/en/articles/7168969-merchandising-search-personalization-guide) Ranking and Personalization - manage how results are ranked,
* Synonyms, Redirects, and other search features are also managed through Nosto Dashboard ([my.nosto.com](https://my.nosto.com/)).
