# Nosto React

[Nosto React](https://github.com/Nosto/nosto-react) is a React component library to make it even easier to implement Nosto. The library provides you with everything to get started with personalization on your React site.

## Why?

You should be using Nosto React if you want to:

- Design personalization experiences with best practices
- Customize your components at will and with ease
- Follow React principles

## Feature list

Our React component library includes the following features:

- Recommendations, including client-side rendering
- Onsite content personalization
- Dynamic bundles
- Debug toolbar\* (excluding advanced use cases)
- Pop-ups & personalised emails
- A/B testing
- Segmentation and Insights
- Analytics
- Search\*\* (when implemented via our code editor)

_\*Note: Our React component library currently does not support advanced use cases of the debug toolbar, but we are constantly working to improve our library and provide you with the best possible integration options. We support most use-cases with page tagging and debug workflows._

_\*\*Note: The search feature is available when implemented via our code editor._

### Additional features

- Lightweight. Almost zero bloat.
- Full support for the Facebook Pixel and Google Analytics.
- Full support for leveraging overlays.
- Full support for the JS API.
- Full support for placements.

## Getting Started

### The root widget

There’s one very specific widget in Nosto React and it is the `NostoProvider` one.

This widget is what we call the Nosto root widget, which is responsible for adding the actual Nosto script and the JS API stub. This widget wraps all other React Nosto widgets. Here’s how:

```jsx
import { NostoProvider } from "@nosto/nosto-react"

<NostoProvider account="your-nosto-account-id" recommendationComponent={<NostoSlot />}>
  <App />
</NostoProvider>
```

**Note:** the component also accepts a prop to configure the host `host="connect.nosto.com"`. In advanced use-cases, the need to configure the host may surface.

### Client side rendering for recommendations

In order to implement client-side rendering, the <NostoProvider> requires a designated component to render the recommendations provided by Nosto. This component should be capable of processing the JSON response received from our backend. Notice the `recommendationComponent={<NostoSlot />}` prop passed to `<NostoProvider>` above.

Learn more [here](https://github.com/Nosto/shopify-hydrogen/blob/main/README.md#client-side-rendering-for-recommendations) and see a [live example](https://github.com/Nosto/shopify-hydrogen-demo) on our demo store.

### Understanding Placements

Nosto React has a special component called `NostoPlacement`. The component is a simply a <u>hidden</u> `<div>` placeholder into which Nosto injects recommendations or personalises the content between the tags.

We recommend adding as many placements across your views as needed as these are hidden and only populated when a corresponding campaign (targeting that placement) is configured.

### Managing the session

Nosto React requires that you pass it the details of current cart contents and the details of the currently logged-in customer, if any, on every route change. This makes it easier to add attribution.

The `NostoSession` component makes it very easy to keep the session up to date so long as the cart and the customer are provided.

The `cart` prop requires a value that adheres to the type `Cart`, while the `customer` prop requires a value that adheres to the type `Customer`.

```jsx
import { NostoSession } from "@nosto/nosto-react"

<>
  <Meta />
  <header>
    <MainMenu />
    <NostoSession cart={currentCart} customer={currentUser} />
  </header>
  <Routes />
  <Footer />
</>
```

## Adding Personalization

Nosto React ships with canned components for the different page types. Each component contains all lifecycle methods to dispatch the necessary events.

### Personalising your home page

The `NostoHome` component must be used to personalise the home page. The component does not require any props.

By default, your account, when created, has <u>four</u> front-page placements named `frontpage-nosto-1`, `frontpage-nosto-2`, `frontpage-nosto-3` and `frontpage-nosto-4`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

The `<NostoHome \>` component needs to be added after the placements. Content and recommendations will be rendered through this component.

```jsx
import { NostoHome, NostoPlacement } from "@nosto/nosto-react"

<div className="front-page">
  ... ... ...
  <NostoPlacement id="frontpage-nosto-1" />
  <NostoPlacement id="frontpage-nosto-2" />
  <NostoPlacement id="frontpage-nosto-3" />
  <NostoPlacement id="frontpage-nosto-4" />
  <NostoHome />
</div>
```

### Personalising your product pages

The `NostoProduct` component must be used to personalise the product page. The component requires that you provide it the identifier of the current product being viewed.

By default, your account, when created, has <u>three</u> product-page placements named `productpage-nosto-1`, `productpage-nosto-2` and `productpage-nosto-3`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

The `<NostoProduct \>` component needs to be added after the placements. Content and recommendations will be rendered through this component. Pass in the product ID via the `product` prop to pass this information back to Nosto.

```jsx
import { NostoPlacement, NostoProduct } from "@nosto/nosto-react"

<div className="product-page">
  ... ... ...
  <NostoPlacement id="productpage-nosto-1" />
  <NostoPlacement id="productpage-nosto-2" />
  <NostoPlacement id="productpage-nosto-3" />
  <NostoProduct product={product.id} />
</div>
```

### Personalising your search result pages

You can personalise your search pages by using the `NostoSearch` component. The component requires that you provide it the current search term.

By default, your account, when created, has <u>two</u> search-page placements named `searchpage-nosto-1` and `searchpage-nosto-2`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

```jsx
import { NostoPlacement, NostoSearch } from "@nosto/nosto-react"

<div className="search-page">
  ... ... ...
  <NostoPlacement id="searchpage-nosto-1" />
  <NostoPlacement id="searchpage-nosto-2" />
  <NostoSearch query={search} />
</div>
```

**Note:** Do not encode the search term in any way. It should be provided an un-encoded string. A query for "black shoes" must be provided as-is and not as "black+shoes". Doing so will lead to invalid results.

### Personalising your category list pages

You can personalise your category and collection pages by using the `NostoCategory` component. The component requires that you provide it the the slash-delimited slug representation of the current category.

By default, your account, when created, has <u>two</u> category placements named `categorypage-nosto-1` and `categorypage-nosto-2`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

```jsx
import { NostoCategory, NostoPlacement } from "@nosto/nosto-react"

<div className="category-page">
  ... ... ...
  <NostoPlacement id="categorypage-nosto-1" />
  <NostoPlacement id="categorypage-nosto-2" />
  <NostoCategory category={category.name} />
</div>
```

**Note:** Be sure to pass in the correct category representation. If the category being viewed is `Mens >> Jackets`, you must provide the name as `/Mens/Jackets` . You must ensure that the category path provided here matches that of the categories tagged in your products.

### Personalising your cart checkout pages

You can personalise your cart and checkout pages by using the `NostoCheckout` component. The component does not require any props.

By default, your account, when created, has <u>two</u> cart-page placements named `categorypage-nosto-1` and `categorypage-nosto-2`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

```jsx
import { NostoCheckout, NostoPlacement } from "@nosto/nosto-react"

<div className="checkout-page">
  ... ... ...
  <NostoPlacement id="checkout-nosto-1" />
  <NostoPlacement id="checkout-nosto-2" />
  <NostoCheckout />
</div>
```

### Personalising your 404 error pages

You can personalise not found pages by using the `Nosto404` component. The component does not require any props.

By default, your account, when created, has three 404-page placements named `notfound-nosto-1`, `notfound-nosto-2` and `notfound-nosto-3`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

```jsx
import { Nosto404, NostoPlacement } from "@nosto/nosto-react"

<div className="notfound-page">
  ... ... ...
  <NostoPlacement id="notfound-nosto-1" />
  <NostoPlacement id="notfound-nosto-2" />
  <NostoPlacement id="notfound-nosto-3" />
  <Nosto404 />
</div>
```

### Personalising your miscellaneous pages

You can personalise your miscellaneous pages by using the `NostoOther` component. The component does not require any props.

By default, your account, when created, has two other-page placements named `other-nosto-1` and `other-nosto-2`. You may omit these and use any identifier you need. The identifiers used here are simply provided to illustrate the example.

```jsx
import { NostoOther, NostoPlacement } from "@nosto/nosto-react"

<div className="other-page">
  ... ... ...
  <NostoPlacement id="other-nosto-1" />
  <NostoPlacement id="other-nosto-2" />
  <NostoOther />
</div>
```

### Personalising your order confirmation page

You can personalise your order-confirmation/thank-you page by using the `NostoOrder` component. The component requires that you provide it with the details of the order.

By default, your account, when created, has one other-page placement named `thankyou-nosto-1`. You may omit this and use any identifier you need. The identifier used here is simply provided to illustrate the example.

```jsx
import { NostoOrder, NostoPlacement } from "@nosto/nosto-react"

<div className="thankyou-page">
  ... ... ...
  <NostoPlacement id="thankyou-nosto-1" />
  <NostoOrder order={order} />
</div>
```

## Hook alternatives

For all the page type specific components hooks are also provided with the same props

- `useNosto404`
- `useNostoOther`
- `useNostoCheckout`
- `useNostoProduct`
- `useNostoCategory`
- `useNostoSearch`
- `useNostOrder`
- `useNostoHome`

## Detailed technical documentation

Find our latest technical specs and documentation hosted [here](https://nosto.github.io/nosto-react).

### Feedback

If you've found a feature missing or you would like to report an issue, simply [open up an issue](https://github.com/nosto/nosto-react/issues/new) and let us know.

We're always collecting feedback and learning from your use-cases. If you find your self customising widgets and forking the repository to make patches - do drop a message. We'd love to know more and understand how we can make React Nosto an even slicker library for you.

### Contributing

Please take a moment to review the guidelines for contributing.

### License

BSD 3-Clause License