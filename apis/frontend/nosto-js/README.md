# Nosto JS

[Nosto JS](https://github.com/Nosto/nosto-js) is a wrapper for the client script, providing an easy way to interact with the Nosto service from your JavaScript or TypeScript applications.

For more information about JS API, see [Our documentation](https://docs.nosto.com/techdocs/apis/frontend/js-apis).

## Installation

To install the package, use your preferred package manager:

```bash
yarn add @nosto/nosto-js
# or
npm install @nosto/nosto-js --save
```

## Modules

| Module | Description |
|-|-|
| . | Core functionality |
| ./client | Client Script types |
| ./performance | Performance tracking utils |
| ./testing | Mocking utils |

## Nosto stub

When using this library, it is not necessary to create the Nosto stub. It will be created automatically as soon as the library is imported for the first time.

## Initialization

The `init` function will load the latest version of the client script and add it to your page on runtime.

```js
import { init } from "@nosto/nosto-js"

await init({
  merchantId: "shopify-12345"
})
```

It returns a promise that resolves when the client script is fully loaded and initialized. However, awaiting it is optional.

## API access

Use the `nostojs` function exported from the library in the same way you would use `window.nostojs`. In fact, this is a typed wrapper over the window function.

```js
import { nostojs } from "@nosto/nosto-js"

await nostojs(async api => {
  const recommendations = await api.loadRecommendations()
  console.info(recommendations)
})
```

You may safely use this even before the `init` function has been called. Any early calls will be queued and processed later, when the client script is initialized.

## Mocking

Use the `mockNostojs` function to mock the Nosto API in unit tests. The function accepts partial API mocks and merges them with default mock API behaviour:

```js
import { mockNostojs } from "@nosto/nosto-js/testing"
import dummyTaggging from "./dummyTagging"

describe("Nosto integration", () => {

  beforeAll(() => {
    mockNostojs({
      pageTagging: () => dummyTaggging
    })
  })

})
```