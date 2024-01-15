# Implementing Autocomplete

Autocomplete is an element shown under search input used to display keywords and products for a partial query.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-08-04 at 12.36.22.png" alt=""><figcaption><p>Example Autocomplete</p></figcaption></figure>

Check out [autocomplete's look & feel guidelines](https://help.nosto.com/en/articles/7169076-autocomplete-s-look-feel-guidelines).

## Configuration

To enable autocomplete, additional configuration should be passed to `init` function.

{% code title="index.js" %}
```javascript
import { init } from '@nosto/preact'

import autocompleteComponent from './autocomplete'
import historyComponent from './history'

init({
    ...window.nostoTemplatesConfig,
    historyComponent,
    autocompleteComponent,
    inputCssSelector: '#search',
    autocompleteQuery: {
        name: 'autocomplete',
        products: {
            size: 5,
        },
        keywords: {
            size: 5,
            fields: [
                'keyword', '_highlight.keyword'
            ],
        },
    }
})
```
{% endcode %}

### Autocomplete component

{% code title="autocomplete/index.jsx" %}
```jsx
import { useAppState, AutocompleteElement } from '@nosto/preact'

export default () => {
    const { response: { products, keywords } } = useAppState()

    if (!products?.hits?.length && !keywords?.hits?.length) {
        return
    }

    return (
        <div>
            {keywords?.hits?.length > 0 && <div>
                <div>
                    Keywords
                </div>
                <div>
                    {keywords.hits.map((hit) => (
                        <AutocompleteElement hit={hit} key={hit.keyword}>
                            {
                                hit?._highlight?.keyword
                                    ? <span dangerouslySetInnerHTML={{ __html: hit._highlight.keyword }}></span>
                                    : <span>{hit.keyword}</span>
                            }
                        </AutocompleteElement>
                    ))}
                </div>
            </div>}
            {products?.hits?.length > 0 && <div>
                <div>
                    Products
                </div>
                <div>
                    {products.hits.map((hit) => (
                        <AutocompleteElement hit={hit} key={hit.productId}>
                            <img src={hit.imageUrl}/>
                            <div>
                                {hit.name}
                            </div>
                            <button
                                // Allow the button to be clicked only once
                                disabled={addedToCart}
                                // Add the product to the cart when the button is clicked
                                onClick={(event) => {
                                    // Don't navigate to the product page
                                    event.stopImmediatePropagation()
                                    event.preventDefault()

                                    // Update the button text and disable it
                                    setAddedToCart(true)

                                    // Add the product to the cart, this depends on the cart implementation
                                    jQuery.post("/cart/add.js", {
                                        quantity: 1,
                                        id: product.productId
                                    })
                                }}
                            >
                                // Show different text if product was added to the cart
                                {addedToCart ? "Added to cart" : "Add to cart"}
                            </button>
                        </AutocompleteElement>
                    ))}
                </div>
            </div>}
            <div>
                <button type="submit">
                    See all search results
                </button>
            </div>
        </div>
    )
}
```
{% endcode %}

#### Element selection

Wrap each keywords and product to `AutocompleteElement` element - it will allow clicking or selecting the element directly with keyboard.&#x20;

#### Search submit

To submit a search directly from the autocomplete, use the `<button type="submit">` element. This will submit the search form.

### History component

History component renders user search history. It is displayed when user clicks on empty search box.

{% code title="history/index.jsx" %}
```jsx
import { useAppState, HistoryElement } from '@nosto/preact'

export default () => {
    const { historyItems } = useAppState()

    if (!historyItems || !historyItems.length) {
        return
    }

    return (
        <div>
            <div>Recently Searched</div>`
            <div>
                {historyItems.map((item) => (
                    <HistoryElement query={{ query: item }}>
                        {item}
                    </HistoryElement>
                ))}
            </div>
        </div>
    )
}
```
{% endcode %}

`HistoryElement` renders clickable element that triggers search event with provided query. &#x20;

## Analytics

Autocomplete automatically tracks to Google Analytics & Nosto Analytics when using `<AutocompleteElement />` component.&#x20;

