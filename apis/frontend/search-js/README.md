# Search JS

[Search JS](https://github.com/Nosto/search-js) is a wrapper for the Nosto Search functionality with some extended functionality such as
* Nosto currency formatting
* Nosto product thumbnails
* Retry logic

For @nosto/search-js specific API documentation, see [Our Typedoc](https://nosto.github.io/search-js/).

For more information about Nosto platform, see [Our documentation](https://docs.nosto.com/techdocs).

For sources, issues and contributions, see the [GitHub repository](https://github.com/Nosto/search-js).

## Installation

To install the package, use your preferred package manager:

```bash
yarn add @nosto/search-js
# or
npm install @nosto/search-js --save
```

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
    ]
})

```