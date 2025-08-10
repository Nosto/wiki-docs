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
    ]
})

```