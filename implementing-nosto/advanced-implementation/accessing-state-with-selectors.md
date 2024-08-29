# Accessing state with selectors

In Nosto, accessing the app state is simple. `useAppState` hook provides you full access to the entire application state.

{% code title="State example" %}
```jsx
import { useAppState } from '@nosto/preact'

export default () => {
    const state = useAppState()
	...
}
```
{% endcode %}

While simple, this approach has a number of disadvantages. Namely, any change to the state will trigger a re-render on every component that uses the state; and it may be awkward to access data that does not reside in a top level property.

{% code title="Example" %}
```jsx
const { response: { products }} = useAppState()
```
{% endcode %}

## Selector hook

The solution to the issues describe above is the `useAppStateSelector` hook. If you have used selectors from other popular state management libraries, then you are already familiar with the concept.

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

The code above is valid and does not have any issues. However, Nosto provides a shorthand for this case, allowing you to select multiple properties from the object a bit easier.

{% code title="Example" %}
```jsx
import { useAppStateSelector, pick } from "@nosto/preact"
...
const { query, response } = useAppStateSelector(state => pick(state, "query", "response"))
```
{% endcode %}

{% hint style="info" %}
The Pick utility is fully type-safe and independent from proprietary logic, allowing you to freely use it on any object in your code.
{% endhint %}
