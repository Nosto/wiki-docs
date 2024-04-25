# Initialization

After installation, you can import and use the library in your JavaScript or TypeScript files. Library attaches to existing search input element. Once a search input is loaded, `autocomplete` function can initialize event listeners for the Search Autocomplete.

❗`autocomplete` should be called once.❗

```jsx
import { useEffect } from "react"
import {
    autocomplete,
    search,
    Autocomplete,
} from "@nosto/autocomplete/react"
import { createRoot } from "react-dom/client"
import "@nosto/autocomplete/styles.css"

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

| Property         | Required | Default Value                                                    | Description                                                                                                                  |
| ---------------- | -------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| inputSelector    | Yes      | N/A                                                              | Input element to attach the autocomplete to                                                                                  |
| dropdownSelector | Yes      | N/A                                                              | Dropdown element to attach the autocomplete to (empty container's selector should be provided)                               |
| render           | Yes      | N/A                                                              | Function to render the dropdown                                                                                              |
| fetch            | Yes      | N/A                                                              | Function to fetch the search state                                                                                           |
| submit           | No       | Search API request                                               | Function to submit the search                                                                                                |
| minQueryLength   | No       | `2`                                                              | Minimum length of the query before searching (applied on typing in autocomplete and used in default `submit` implementation) |
| historyEnabled   | No       | `true`                                                           | Enables search history component                                                                                             |
| historySize      | No       | `5`                                                              | Max number of history items to show                                                                                          |
| nostoAnalytics   | No       | `true`                                                           | Enable Nosto Analytics                                                                                                       |
| googleAnalytics  | No       | `{ serpPath: "search", queryParamName: "query", enabled: true }` | Google Analytics configuration. Set to `false` to disable                                                                    |

