# Search JS

[Search JS](https://github.com/Nosto/search-js) is a wrapper for the Nosto Search functionality with some extended functionality such as
* Nosto currency formatting
* Nosto product thumbnails
* Retry logic

## Nosto stub

When using this library, it is not necessary to create the Nosto stub. It will be created automatically as soon as the library is imported for the first time.

## Usage

The main export of this library is the `search` function. It is compatible with the search function of the Nosto JS API and adds a couple of additional options

```ts
import { search } from "@nosto/search-js"
import { priceDecorator } from "@nosto/search-js/currencies"
import { thumbnailDecorator } from "@nosto/search-js/thumbnails"

const response = await search({
    query: 'my search',
    products: { 
        fields: [
            "productId",
            "name",
            "price",
            "listPrice",
            "priceCurrencyCode",
            "imageUrl",
            "imageHash"
        ] 
    }
}, {
    track: 'serp',
    hitDecorators: [
        priceDecorator(),
        thumbnailDecorator({ size: "9" })
    ],
    maxRetries: 3,
    retryInterval: 2000,
    usePersistentCache: true,
    useMemoryCache: false
})

```

## Search Options

The search function accepts all standard Nosto JS API options plus the following search-js specific options:

### `hitDecorators`
**Type:** `HitDecorator[]`  
**Default:** `undefined`

Hit decorators to apply to the search results. These allow you to transform search results with additional functionality like currency formatting and thumbnail generation.

```ts
import { priceDecorator } from "@nosto/search-js/currencies"
import { thumbnailDecorator } from "@nosto/search-js/thumbnails"

const response = await search(searchQuery, {
    hitDecorators: [
        priceDecorator(),
        thumbnailDecorator({ size: "9" })
    ]
})
```

You can also create custom decorators to add additional fields to your search results:

```ts
import { SearchProduct } from "@nosto/nosto-js/client"

type DecoratedProduct = SearchProduct & { handle?: string }

export function handleDecorator(product: SearchProduct): DecoratedProduct {
  if (product.url) {
    const pathname = new URL(product.url).pathname
    return {
      ...product,
      handle: pathname.split("/").pop() || undefined
    }
  }
  return product
}

const response = await search(searchQuery, {
    hitDecorators: [
        priceDecorator(),
        thumbnailDecorator({ size: "9" }),
        handleDecorator
    ]
})
```

### `maxRetries`
**Type:** `number`  
**Default:** `0`

Maximum number of retry attempts when a search request fails. Setting this to `0` disables retry logic.

```ts
const response = await search(searchQuery, {
    maxRetries: 3 // Will retry up to 3 times on failure
})
```

### `retryInterval`
**Type:** `number`  
**Default:** `1000`

Interval (in milliseconds) between retry attempts when a search request fails.

```ts
const response = await search(searchQuery, {
    maxRetries: 3,
    retryInterval: 2000 // Wait 2 seconds between retries
})
```

### `usePersistentCache`
**Type:** `boolean`  
**Default:** `false`

Whether to use a persistent cache for the search results. When enabled, search results are cached across browser sessions. Only a single result will be cached and this feature is primarily meant to support efficient back navigation from product pages to search results.

```ts
const response = await search(searchQuery, {
    usePersistentCache: true // Cache results persistently
})
```

### `useMemoryCache`
**Type:** `boolean`  
**Default:** `false`

Whether to use an in-memory cache for search results. When enabled, search results are cached in memory for the current session. Memory cache can be considered for autocomplete results to improve performance.

```ts
const response = await search(searchQuery, {
    useMemoryCache: true // Cache results in memory
})
```