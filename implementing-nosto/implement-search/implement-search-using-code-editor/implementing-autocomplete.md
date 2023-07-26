# Implementing Autocomplete

Autocomplete is an element shown under search input used to display products for a partial query.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-03-31 at 17.28.40.png" alt=""><figcaption><p>Example Autocomplete</p></figcaption></figure>

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
    const { response: { products } } = useAppState()

    if (!products?.hits?.length) {
        return
    }

    return (
        <div>
            <div>
                Products
            </div>
            <div>
                {products.hits.map((hit) => (
                    <AutocompleteElement hit={hit}>
                        <img src={hit.imageUrl}/>
                        <div>
                            {hit.name}
                        </div>
                    </AutocompleteElement>
                ))}
            </div>
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

#### Product selection

Wrap each product to `AutocompleteElement` element - it will allow clicking or selecting the product directly with keyboard.&#x20;

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

