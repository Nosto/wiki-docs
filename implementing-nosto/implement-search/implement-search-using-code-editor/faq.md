# FAQ

## Share search templates code between multiple accounts

The functionality enables the sharing of a single instance of search templates across multiple dashboard accounts through linking. This feature proves particularly advantageous in scenarios where there are multiple websites with identical or similar characteristics that necessitate the creation of multiple dashboard accounts (such as supporting multiple languages or environments).

{% hint style="info" %}
To link multiple accounts contact support!
{% endhint %}

### How to implement some differences between websites?

Even if multiple websites share the same search templates code, we can easily have different logic based on website host or language (or other criteria).

```javascript
function detectWebsite() {
    if (location.hostname === 'de.website.com') {
        return 'de'
    } else if (document.documentElement.lang === 'de') {
        return 'de'
    }
    return 'en'
}

const helloText = {
    de: 'Hallo',
    en: 'Hello'
}[detectWebsite()]
```

### Different CSS between websites

Since it's not possible to target CSS rules by domain, it's recommended to add a special class to the page body and use it to target CSS rules.

#### Example JS

```javascript
init({
    ...
}).then(() => {
    if (location.hostname === 'de.website.com') {
        document.body.classList.add('nosto-search-de')
    }
})
```

#### Example CSS:

```css
.nosto-search-de .my-title {
    color: black;
}
```

## Show specific SKU when searching by SKU color or ID

By default, the search indexes all SKUs as a single product. Therefore, even when searching by a specific SKU attribute (like SKU color or ID), the search will return the main product along with all SKUs.

However, because the search returns all SKUs, SKU selection logic can be implemented on the frontend side. This is achieved by checking each SKU to see if it contains text from the search query.

### Implementation

These is an example on how to implement SKU selection:

```jsx
function getSku(query, product) {
    const words = query.toLowerCase().split(/\s+/)

    return product?.skus?.find((sku) => {
        // If the query is a SKU ID, return the SKU
        if (sku.id == query) {
            return true
        }
        const color = sku?.customFields?.color?.toLowerCase()?.split(/\s+/)

        return words.some((word) => color.includes(word))
    })
}

export default ({ product }) => {
    const { query: { query }} = useAppState()
    const sku = getSku(query, product)

    return <a href={sku?.url || product.url}>
        <img src={sku?.imageUrl || product.imageUrl}/>

        {sku?.name || product.name}
    </a>
}
```

## Use more than 100 facet values

By default, the search API returns 100 values for each facet, and it's not possible to control that limit in the dashboard. However, it is possible to override [the facet configuration](https://search.nosto.com/v1/graphql?ref=InputSearchFacetConfig) directly from the code.

### Get facet ID from the dashboard

The first step is to know the facet ID of the facet you want to overwrite. The facet ID is stored in the dashboard URL. For example, if you open the facet in the facet manager, the URL should be `https://my.nosto.com/admin/shopify-123/search/settings/facetManager/6406df867f8beb629fc0dfb9`. This means the facet ID is `6406df867f8beb629fc0dfb9`.

### Override facet settings

Once the ID is known, it's possible to override any facet setting by specifying overwrite properties in the `customFacets` parameter:

{% code title="index.js" %}
```javascript
init({
    serpQuery: {
        products: {
            customFacets: [
                {
                    "id": "6406df867f8beb629fc0dfb9",
                    "size": 10
                }
            ]
        },
    }
})
```
{% endcode %}

## Search returns up to 10000 documents

Due to performance optimization, the search function will calculate the total count up to 10,000. In this case the search page should display a count of `10,000+` to indicate that more than 10,000 products were found.

Filters and sorting operations are executed on all found products, even if there are more than this limit. Therefore, it's still possible to find other products if you filter or sort them. This should not affect the user experience in any way because it's unlikely that someone would actually view more than 10,000 products with a single search.

## SEO

Although search engines can understand some JavaScript-rendered code, they often miss search templates. The reason for this is that the rendering of search templates is delayed in order not to hinder the page loading speed.

However, it's still possible to achieve great SEO results while using the code editor:

* **Category pages** - Ensure that the category page already returns the correct meta tags and page title according to [SEO recommendations](https://developers.google.com/search/docs/fundamentals/seo-starter-guide), they bring the biggest impact to the SEO. In category page search templates render only products, so it doesn't significantly impact SEO. If you have concerns about search engines not seeing the products, it is possible to add [structured data ](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data)to a page (the category page backend should generate these tags using original products). However, this does not guarantee to boost SEO because search engines may not favor the discrepancy between structured data and the actual rendered products.
* **Search pages** - Most search engines don't index search pages, so no optimizations are needed.

If you still have concerns regarding SEO, please consider using [API integration](https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-api).
