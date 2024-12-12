# Implementing Search page

### Configuration

To create a search application, call the `init` function with your configuration. This will create a new Preact application that renders on the specified `contentCssSelector`. It will also bind to the input element identified by the provided `inputCssSelector` and execute a search upon form submission.

<pre class="language-javascript" data-title="index.js"><code class="lang-javascript"><strong>import { init } from '@nosto/preact'
</strong>
import serpComponent from './serp'

init({
    ...window.nostoTemplatesConfig,
    serpComponent,
    inputCssSelector: '#search',
    contentCssSelector: '#content',
    serpPath: '/search',
    serpPathRedirect: false,
    formCssSelector: '#search-form',
    formUnbindDelay: 1000, // 1 second
    serpUrlMapping: {
        query: 'q',
    },
    serpQuery: {
        products: {
            size: 20,
            from: 0,
        },
    },
})
</code></pre>

#### Serp query parameter flavors&#x20;

In the example above, we supply serp query parameters as an object. Additionally, the `serpQuery` parameter can also be supplied as a function. The function flavor can be used for building complex query parameters and provides access to other pre-defined configuration parameters. Section below shows an example of `serpQuery` supplied as a function which provides the product variation id by accessing the pre-defined `variationId` method from the default configuration.

<pre class="language-javascript" data-title="index.js"><code class="lang-javascript"><strong>import { init } from '@nosto/preact'
</strong>
import serpComponent from './serp'

init({
    ...window.nostoTemplatesConfig,
    serpComponent,
    inputCssSelector: '#search',
    contentCssSelector: '#content',
    serpPath: '/search',
    serpPathRedirect: false,
    formCssSelector: '#search-form',
    formUnbindDelay: 1000, // 1 second
    serpUrlMapping: {
        query: 'q',
    },
    serpQuery() {
        return {
            products: {
                size: 20,
                from: 0,
                variationId: this.variationId()
            },
        }
    }
})
</code></pre>

The full list of Configuration options is documented [here](https://nosto.github.io/search-templates/library/interfaces/Config.html)

### Search page path

When `serpPath` parameter is specified, the application will **redirect to the specified search** page after a search is conducted. Otherwise, the search will be **rendered** **on the same page** without changing the URL path.

### Search page redirect

When `serpPathRedirect` parameter is set to `true`, the application after search submission will redirect the page to the search page specified in `serpPath` and fetch the page. Default behaviour will rewrite browser history only to the specified path, without fetching the page.

### Unbinding existing search input

To prevent events from firing on an existing input, you need to provide the CSS selector of the form that the input is in to the initialization configuration. When optional `fromCssSelector` is passed, it will unbind the form and the elements inside from existing events. Additionally, `formUnbindDelay` in milliseconds as value can be passed to delay the unbinding functionality.

### Serp component

The search results page component should render a full search page using the provided app state. A minimal example might look like this:

{% code title="serp/index.js" %}
```jsx
import { useAppStateSelector, SerpElement } from '@nosto/preact'

export default () => {
    const { products, loading } = useAppStateSelector((state) => ({
        products: state.response.products,
        loading: state.loading,
    }))

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

### Automatic URL Parameter Compression

When the `compressUrlParameters` flag is set to `true`, it automatically applies the URL parameter compression functions for filters, sort and pagination.

<pre class="language-javascript"><code class="lang-javascript"><strong>import { init } from '@nosto/preact'
</strong>
import serpComponent from './serp'

init({
    ...window.nostoTemplatesConfig,
    serpComponent,
    inputCssSelector: '#search',
    contentCssSelector: '#content',
    serpPath: '/search',
    serpPathRedirect: false,
    serpUrlMapping: {
        query: 'q',
        'products.page': 'page'
    },
    compressUrlParameters: true,
    serpQuery: {
        products: {
            size: 20,
            from: 0,
        },
    },
})
</code></pre>

`@nosto/preact` library has pre-built functions for changing search url format:

<table><thead><tr><th width="210"></th><th width="173.33333333333331">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>Pagination</code></td><td>Replaces <code>from</code> parameter with page number.</td><td>Before:<br><code>/search?products.from=20&#x26;q=shorts</code><br><br>After:<br><code>/search?page=2&#x26;q=shorts</code></td></tr><tr><td><code>Sorting</code></td><td>Returns shorter <code>sort</code> parameters.</td><td>Before:<br><code>/search?q=shorts&#x26;products.sort.0.field=price&#x26;products.sort.0.order=desc</code><br><br>After:<br><code>/search?q=shorts&#x26;products.sort=price~desc</code></td></tr><tr><td><code>Filtering</code></td><td>Compresses <code>filter</code> parameters. Multiple <code>filter</code> values are separated by a comma, which is encoded. This is because <code>filter</code> values can contain non-alphanumeric letters themselves.</td><td>Before:<br><code>/search?q=shorts&#x26;products.filter.0.field=customFields.producttype&#x26;products.filter.0.value.0=Shorts&#x26;products.filter.0.value.1=Swim&#x26;products.filter.1.field=price&#x26;products.filter.1.range.0.gte=10&#x26;products.filter.1.range.0.lte=30</code><br><br>After:<br><code>/search?q=shorts&#x26;filter.customFields.producttype=Shorts%7C%7CSwim&#x26;filter.price=10~30</code></td></tr></tbody></table>

### Query parameter mapping

In addition to the `compressUrlParameters` flag the `serpUrlMapping` should be used to control the mapping from URL parameter keys to paths in the internal query object. The default looks like this:

```js
serpUrlMapping: {
  query: "q",
  "products.filter": "filter",
  "products.page": "page",
  "products.sort": "sort"
}
```

The key is the internal path in the query model and the value is the query parameter name that should be used in the URL.

## Features

### Faceted navigation

#### Stats facet

The [stats facet](https://search.nosto.com/v1/graphql?ref=SearchStatsFacet) returns the minimum and maximum values of numerical fields from search results. This functionality is especially useful when creating interactive elements such as sliders and range selectors. For instance, a price slider can use these min-max values to define its adjustable range, providing a simple way for users to filter products within a specific price range. Similarly, these values are utilized in the RangeSelector to define the overall scope of range selections, allowing for the configuration of selection precision through the range size parameter.

**Range Slider**

Utilize the `useRangeSlider` hook to generate useful context for rendering range inputs. Additionally, employ the component to generate the interactive slider itself. These tools together facilitate the creation of dynamic and interactive range sliders for your application.

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

**Range Selector**

If you require an alternative method where values are selected through radio buttons rather than a slider, consider using `useRangeSelector` hook. This tool allows users to choose from predefined range intervals with radio buttons, offering a different interaction style.

The range size parameter in the `useRangeSelector` hook specifies the size of each interval in the range and determines the total number of range items displayed. Additionally, it automatically rounds the minimum value down to ensure intervals are aligned with the specified range size.

For example, if the minimum product price in the current catalog is 230, and the maximum product price is 1000, the range size of 200 will adjust the starting point to 200 and create intervals displayed under the "Price" filter as follows:

* 200 - 400
* 400 - 600
* 600 - 800
* 800 - 1000

```jsx
import { useRangeSelector } from "@nosto/preact"
import { useState } from "preact/hooks"
import RangeInput from "./elements/RangeInput"
import Icon from "./elements/Icon"
import RadioButton from "./elements/RadioButton"

export default function RangeSelector({ facet }) {
    const {
        min,
        max,
        range,
        ranges,
        updateRange,
        handleMinChange,
        handleMaxChange,
        isSelected
    } = useRangeSelector(facet.id, 100)
    const [active, setActive] = useState(false)

    return (
        <li>
            <div>
                <ul>
                    {ranges.map(({ min, max, selected }, index) => {
                        return (
                            <li
                            >
                                <RadioButton
                                    key={index}
                                    value={`${min} - ${max}`}
                                    selected={selected}
                                    onChange={() => updateRange([min, max])}
                                />
                            </li>
                        )
                    })}
                    <div>
                        <div>
                            <label for={`ns-${facet.id}-min`}>
                                Min.
                            </label>
                            <RangeInput
                                id={`ns-${facet.id}-min`}
                                min={min}
                                max={max}
                                range={range}
                                value={range[0] ?? min}
                                onChange={e => handleMinChange(parseFloat(e.currentTarget.value) || min)}
                            />
                        </div>
                        <div>
                            <label for={`ns-${facet.id}-max`}>
                                Max.
                            </label>
                            <RangeInput
                                id={`ns-${facet.id}-max`}
                                min={min}
                                max={max}
                                range={range}
                                value={range[1] ?? max}
                                onChange={e => handleMaxChange(parseFloat(e.currentTarget.value) || max)}
                            />
                        </div>
                    </div>
                </ul>
            </div>
        </li>
    )
}
```

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

You can use the `toggleProductFilter` function to toggle any filter value. This function will either add the filter value if it's not already applied or remove it if it's currently active, thus providing an efficient way to manipulate product filters in your application.

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

#### Infinite Scroll

Nosto search-templates library provides a simple out-of-the-box solution to implement infinite scroll functionality. Simply wrapping your product rendering with the `<InfiniteScroll>` component is generally enough.

As the user scrolls the page down, the wrapper will detect it using the `IntersectionObserver`. If it is not supported by the user's browser, a 'load more' button will be shown instead.

{% code title="serp.jsx" %}
```jsx
function Products() {
    const products = useAppStateSelector(state => state.response.products)

    return (
        <>
            {products.hits.map((hit, index) => {
                return <Product product={hit} key={hit.productId ?? index} />
            })}
        </>
    )
}

function SerpInfiniteScroll() {
    return (
        <InfiniteScroll>
            <Products />
        </InfiniteScroll>
    )
}
```
{% endcode %}

#### Persistent Search Cache

When using infinite scroll, consider enabling persistent search cache as well. When this feature is enabled, the latest search API response will be automatically cached and stored in the browser's session storage.

This improves the user experience significantly when the user navigates from a product details page back into the search results using the browser's 'back' function. The data necessary to display the products is already available, and the user will see the products immediately, without waiting for them to load again.

This feature is useful for both paginated and infinite scroll, but the benefits are significantly more visible with the latter.

{% code %}
```jsx
import { init } from '@nosto/preact'

init({
    ...otherFields,
    persistentSearchCache: true,
})
```
{% endcode %}

### Product actions

Since the code editor utilizes the Preact framework, it offers significant flexibility in customizing behavior or integrating the search page with existing elements on your site. For instance, you can implement actions such as 'Add to Cart', 'Wishlist', or 'Quick View'.

{% code title="serp/Product.jsx" %}
```jsx
import { SerpElement } from '@nosto/preact'
import { useState } from 'preact/hooks'

export default ({ product }) => {
    const [addedToCart, setAddedToCart] = useState(false)
    
    return (
        <SerpElement
            as="a"
            hit={product}
        >
            <img src={product.imageUrl} />
            <div>
                {product.name}
            </div>
            <button
                // Allow the button to be clicked only once
                disabled={addedToCart}
                // Add the product to the cart when the button is clicked
                onClick={(event) => {
                    // Don't navigate to the product page
                    event.preventDefault()

                    // Update the button text and disable it
                    setAddedToCart(true)

                    // Add the product to the cart, this depends on the cart implementation
                    jQuery.post('/cart/add.js', {
                        quantity: 1,
                        id: product.productId,
                    })
                }}
            >
                // Show different text if product was added to the cart
                {addedToCart ? 'Added to cart' : 'Add to cart'}
            </button>
        </SerpElement>
    )
}
```
{% endcode %}

### Handling native results

Nosto will attempt to display the original search results in case Nosto service is unavailable or can't be reached. In addition, the original products are made available for the SEO crawlers, improving the page's ranking in the search engines. To make it possible, it's recommended to hide the original search results instead of removing or blocking them.

The best approach is to add `ns-content-hidden` class name to the same element you are targeting with `contentCssSelector` or `categoryCssSelector`. This class name will be stripped away by Nosto automatically as soon as the script is initialized.

In addition, you should define CSS to hide the target element:

{% code title="css" %}
```css
.ns-content-hidden {
    display: none;
    /* Or other styles as needed */
}
```
{% endcode %}

## Analytics

Search automatically tracks to Google Analytics & Nosto Analytics when using `SerpElement` component.

```jsx
export default ({ product }) => {
    return (
        <SerpElement as="a" hit={product}>
            {product.name}
        </SerpElement>
    )
}
```

**Component parameters:**

| **hit**                | Product object.                                                                                                                                                                                              |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **as**                 | <p>Element to render <code>&#x3C;SerpElement /></code> as. Recommended to use <strong>as="a"</strong>.<br>If <strong>a</strong> element is used, <strong>href</strong> attribute is added automatically.</p> |
| **onClick** (optional) | Additional onClick callback (tracking callback is already implemented in the component).                                                                                                                     |

{% hint style="info" %}
The `SerpElement` component supports any other HTML attribute, e.g. **class.**
{% endhint %}

## Fallback Functionality

The search page incorporates built-in fallback functionality, allowing users to customize the behavior in case the search service encounters issues. To activate this feature, modify the initialization configuration to include the `fallback: true` key-value pair.

### Enabling Fallback

To enable fallback functionality, include the following code in the initialization configuration:

```javascript
import { init } from '@nosto/preact'

init({
    ...window.nostoTemplatesConfig,
    fallback: true,
})
```

Once fallback is enabled, if the search request fails to retrieve data, the search functionality will be temporarily disabled for 10 minutes, and the original content Nosto has overridden will be restored.

{% hint style="info" %}
The `fallback: true` setting only works out of the box if the path is the same for both the dedicated Nosto search page and the native search page, as well as for category pages.

If the paths differ, you must configure the `serpFallback` or `categoryFallback` function to ensure proper redirection. See: [Customizing Fallback Location](https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-code-editor/implementing-search-page#customizing-fallback-location)
{% endhint %}


### Alternative Fallback Behaviour

If the behaviour described above is undesirable, the configuration supports an alternative option. Fallback mode can be set to `fallback: 'legacy'`, in which case the user will see a page reload if the search request fails. After that, Nosto will not attempt to override the original search results or category pages for 10 minutes.

{% hint style="info" %}
This behaviour has been the default fallback behaviour before August 20, 2024.
{% endhint %}

```javascript
import { init } from '@nosto/preact'

init({
    ...window.nostoTemplatesConfig,
    fallback: 'legacy',
})
```

### Customizing Fallback Location

Additionally, it's possible to customize the location to which users are redirected when the search functionality is unavailable. This customization involves specifying functions for both the search engine results page (SERP) and category pages.

#### **SERP Fallback**

To redirect users to a specific location when the search engine is down, define a function for `serpFallback`. This function accepts one parameter containing information about the current search query, including the query itself.

```javascript
import { init } from '@nosto/preact'

init({
    ...window.nostoTemplatesConfig,
    fallback: true,
    serpFallback: (searchQuery) => {
        location.replace(`/search?q=${searchQuery.query}`);
    },
})
```

#### Category Fallback

Similarly, for category pages, define a function for `categoryFallback`. This function also accepts one parameter containing information about the current query, including the category ID or Path.

```javascript
import { init } from '@nosto/preact';

init({
    ...window.nostoTemplatesConfig,
    fallback: true,
    categoryFallback: (query) => {
        location.replace(`/categories/${query.products.categoryId}`);
    },
});
```

By customizing these fallback locations, you can enhance the user experience by providing them with alternative navigation options if the search functionality is temporarily unavailable.

## Search engine configuration <a href="#selecting-fields" id="selecting-fields"></a>

Nosto Search engine is relevant out of the box and search API can be used without any initial setup. Nosto Dashboard can be used to further tune search engine configuration:

* [Searchable Fields](https://help.nosto.com/en/articles/7161528-search-engine-s-logic-and-searchable-fields) - manage which fields are used for search and their priorities,
* [Facets](https://help.nosto.com/en/articles/7169091-setting-up-facets) - create facets (filtering options) for search results page,
* [Ranking and Personalization](https://help.nosto.com/en/articles/7168969-merchandising-search-personalization-guide) Ranking and Personalization - manage how results are ranked,
* Synonyms, Redirects, and other search features are also managed through Nosto Dashboard ([my.nosto.com](https://my.nosto.com/)).
