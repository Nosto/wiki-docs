All product pages should contain the product tagging. The product tagging can be the entire metadata or only a small subset of it.

The product tagging is used to pass the context of the current product being viewed which in turn is used to personalise the recommendations e.g. cross-sellers, and commonly also periodically crawled by Nosto to build an index.

**Note:** The product tagging _must_ be server-side rendered as the Nosto crawler does not execute Javascript.

You can tag your product pages in two different ways:

1. [Tagging all the metadata (Recommended):](https://github.com/Nosto/techdocs/wiki/Basic:-Default-Product-Tagging) This approach is the recommended way to tag your product pages. It contains the entirety of the product metadata and leverages the crawler to build a 1:1 replica of your catalog.

2. [Tagging the bare minimum:](https://github.com/Nosto/techdocs/wiki/Basic:-Minimum-Product-Tagging) This approach entails tagging just the product-id and requires you to leverage an API to build a 1:1 replica of your catalog. This is an advanced use-case and requires that your account-manager disables crawling for your website.