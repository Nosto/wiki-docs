# Usage with Next.js

Due to the nature of Next.js being a server side rendering framework, using nosto-autocomplete with Next.js requires a few special considerations.

## Client-side components

Next.js is very particular about how and when the components are rendered. Many parts of the Nosto ecosystem rely on the `window` object being available, which is not the case in the `SSR` environment. For best results, we recommend rendering the nosto-autocomplete as a dynamic client side component.

```jsx
import dynamic from 'next/dynamic';

export const ClientOnlySearch = dynamic(() => import('./components/search').then(module => module.Search), {
  ssr: false,
});
```

## Rendering results

Our examples show injecting the results into your page using `React.createRoot`, but with Next.js you will want to avoid that. Instead, you should opt for a React Portal based solution, or just a simple conditional render. Importantly, you should always render the results in the same React context as your main app.

For a simple use case, you could extract the state from the `render` function and render the results in the same component.

```jsx
export function SearchComponent() {
    import { useState, useEffect } from "react"

    const [autocompleteState, setAutocompleteState] = useState()

    useEffect(() => {
        autocomplete({
            ...,
            render: function (_, state) {
                setAutocompleteState(state)
            },
        })
    }, [])

    return (
        <form id="search-form">
            <input type="text" id="search" placeholder="search" />
            <button type="submit" id="search-button">
                Search
            </button>
            <div id="search-results">
                {autocompleteState && <Autocomplete {...autocompleteState} />}
            </div>
        </form>
    )
}
```

If you would like to render the results in a different part of your app, consider using [React Portal](https://react.dev/reference/react-dom/createPortal), notifying the relevant component using an event bus, or saving the extracted state object into your state management solution, such as `Redux`.

## Routing

Another consideration is the routing. The default `Autocomplete` component that nosto-autocomplete exports uses plain `<a href="..." />` tags for navigation, which are not handled by Next.js. Your page will perform a full reload when you navigate over to the autocomplete result if you use the plain anchor tags.

Consider implementing your own version of the Autocomplete component using the `Link href="..."` components from Next.js in place of anchors. If you use wish to use our [Autocomplete component](https://github.com/Nosto/nosto-autocomplete/blob/main/src/defaults/Autocomplete.tsx) as a template, you can make a local copy and replace the link components.