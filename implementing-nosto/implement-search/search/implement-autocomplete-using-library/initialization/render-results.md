# Render results

Once the autocomplete component binds to input via `inputSelector` and `dropdownSelector`, it then renders autocomplete provided in a `render` function. It is called on input focus and change events, and renders a dropdown element with the current search result state:

* if input is empty and history entries exist, it renders dropdown with history list,
* if input is not empty and it passes `minQueryLength` rule, it render dropdown with keywords and products.

Render can be adjusted to the desired framework. Moreover, the library provides helpers for Mustache/Liquid template languages.

**Examples**

Suppose `index.html` is

```html
<form id="search-form">
    <input type="text" id="search" placeholder="search" />
    <button type="submit" id="search-button">Search</button>
    <div id="search-results" className="ns-autocomplete"></div>
</form>
```

List of `autocomplete` initialization examples:

1. **Liquid**\
   Example below uses `fromLiquidTemplate` helper which renders string template. Library provides default autocomplete template via `defaultLiquidTemplate` and default css for default template:

```js
import {
    autocomplete,
    search,
    fromLiquidTemplate,
    defaultLiquidTemplate,
} from "@nosto/autocomplete/liquid"
import "@nosto/autocomplete/styles.css"

autocomplete({
    fetch: {
        products: {
            fields: ["name", "url", "imageUrl", "price", "listPrice", "brand"],
            size: 5,
        },
        keywords: {
            size: 5,
            fields: ["keyword", "_highlight.keyword"],
            highlight: {
                preTag: `<strong>`,
                postTag: "</strong>",
            },
        },
    },
    inputSelector: "#search",
    dropdownSelector: "#search-results",
    render: fromLiquidTemplate(defaultLiquidTemplate),
    submit: async (query, config, options) => {
        if (query.length >= config.minQueryLength) {
            const response = await search(
                {
                    query,
                },
                {
                    redirect: true,
                    track: config.nostoAnalytics ? "serp" : undefined,
                    ...options
                }
            )
            // Do something with response. For example, update Search Engine Results Page products state.
        }
    },
})
```

The template also can be loaded from a file. The library includes a default template, equivalent to string template in above example:

```js
import {
    autocomplete,
    search,
    fromRemoteLiquidTemplate,
} from "@nosto/autocomplete/liquid"
import "@nosto/autocomplete/styles.css"

autocomplete({
    // ...
    render: fromRemoteLiquidTemplate(
        `./node_modules/@nosto/autocomplete/dist/liquid/autocomplete.liquid`
    ),
})
```

2. **Mustache**\
   Mustache template is rendered similarly as Liquid template in the above example:

```js
import {
    autocomplete,
    search,
    fromMustacheTemplate,
    defaultMustacheTemplate,
} from "@nosto/autocomplete/mustache"
import "@nosto/autocomplete/styles.css"

autocomplete({
    // ...
    render: fromMustacheTemplate(defaultMustacheTemplate),
})
```

Or from a file:

```js
import {
    autocomplete,
    search,
    fromRemoteMustacheTemplate,
} from "@nosto/autocomplete/mustache"
import "@nosto/autocomplete/styles.css"

autocomplete({
    // ...
    render: fromRemoteMustacheTemplate(
        `./node_modules/@nosto/autocomplete/dist/mustache/autocomplete.mustache`
    ),
})
```

3. **React/Preact**\
   One way to initialize autocomplete in a React app, is to call `autocomplete` from the `useEffect` on component mount, using default `<Autocomplete />` component and styles:

```jsx
import { useEffect } from "react"
import { createRoot } from "react-dom/client"
import {
    autocomplete,
    search,
    Autocomplete,
} from "@nosto/autocomplete/react"
import "@nosto/autocomplete/styles"

let reactRoot = null

export function Search() {
    useEffect(() => {
        autocomplete({
            fetch: {
                products: {
                    fields: [
                        "name",
                        "url",
                        "imageUrl",
                        "price",
                        "listPrice",
                        "brand",
                    ],
                    size: 5,
                },
                keywords: {
                    size: 5,
                    fields: ["keyword", "_highlight.keyword"],
                    highlight: {
                        preTag: `<strong>`,
                        postTag: "</strong>",
                    },
                },
            },
            inputSelector: "#search",
            dropdownSelector: "#search-results",
            render: function (container, state) {
                if (!reactRoot) {
                    reactRoot = createRoot(container)
                }

                reactRoot.render(<Autocomplete {...state} />)
            },
            submit: async (query, config, options) => {
                if (query.length >= config.minQueryLength) {
                    const response = await search(
                        {
                            query,
                        },
                        {
                            redirect: true,
                            track: config.nostoAnalytics ? "serp" : undefined,
                            ...options
                        }
                    )
                    // Do something with response. For example, update Search Engine Results Page products state.
                }
            },
        })
    }, [])

    return (
        <form id="search-form">
            <input type="text" id="search" placeholder="search" />
            <button type="submit" id="search-button">
                Search
            </button>
            <div id="search-results" className="ns-autocomplete"></div>
        </form>
    )
}
```

The Preact solution does not differ from React a lot:

```jsx
import { render } from "preact/compat"
import { useEffect } from "preact/hooks"
import {
    Autocomplete,
    autocomplete,
    search,
} from "@nosto/autocomplete/preact"
import "@nosto/autocomplete/styles.css"

export function Search() {
    useEffect(() => {
        autocomplete({
            // ...
            render: function (container, state) {
                render(<Autocomplete {...state} />, container)
            },
        })
    }, [])

    return <form id="search-form">{/* ... */}</form>
}
```
