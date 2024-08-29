# Accessing state with selectors

Deprecated `useAppState` hook provides you full access to the entire application state.

While simple, this hook has a number of disadvantages. Namely, any change to the state will trigger a re-render on every component that uses the state; and it may be complicated to access nested data. For better performance and usability, consider the `useAppStateSelector` hook described in the next section

## Selector hook

`useAppStateSelector` hook accepts one argument, which is a function taking a part (also known as a slice) of the state. The hook will then trigger a component render only when this slice changes.

{% code title="Example" %}
```jsx
const products = useAppStateSelector((state) => state.response.products)
```
{% endcode %}

## Selecting multiple top level properties

Sometimes you would want to select multiple properties from the state. It is possible to achieve by returning an object:

{% code title="Example" %}
```jsx
const { query, response } = useAppStateSelector(state => ({
    query: state.query,
    response: state.response
}))
```
{% endcode %}

Nosto also offers a shorthand version for selecting multiple properties, the `Pick` function. With this function, selecting multiple properties from the object becomes easier.

{% code title="Example" %}
```jsx
import { useAppStateSelector, pick } from "@nosto/preact"
...
const { query, response } = useAppStateSelector(state => pick(state, "query", "response"))
```
{% endcode %}

{% hint style="info" %}
The Pick utility is fully independent from proprietary logic, allowing you to freely use it on any object in your code.
{% endhint %}
