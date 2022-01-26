# Online Store 2.0

- [Online Store 2.0](#online-store-20)
  - [Summary](#summary)
  - [Nosto's implementation](#nostos-implementation)
    - [Existing Shopify themes](#existing-shopify-themes)
    - [Online 2.0 themes](#online-20-themes)
      - [Existing JSON templates](#existing-json-templates)
        - [templates/404.json](#templates404json)
        - [templates/cart.json](#templatescartjson)
        - [templates/collection.json](#templatescollectionjson)
        - [templates/index.json](#templatesindexjson)
        - [templates/product.json](#templatesproductjson)
        - [templates/search.json](#templatessearchjson)
      - [New liquid templates](#new-liquid-templates)
        - [sections/nosto-404.liquid](#sectionsnosto-404liquid)
        - [sections/nosto-cart.liquid](#sectionsnosto-cartliquid)
        - [sections/nosto-collection-prepend.liquid](#sectionsnosto-collection-prependliquid)
        - [sections/nosto-collection-append.liquid](#sectionsnosto-collection-appendliquid)
        - [sections/nosto-index.liquid](#sectionsnosto-indexliquid)
        - [sections/nosto-product.liquid](#sectionsnosto-productliquid)
        - [sections/nosto-search-prepend.liquid](#sectionsnosto-search-prependliquid)
        - [sections/nosto-search-append.liquid](#sectionsnosto-search-appendliquid)

## Summary

Shopify has recently released a version 2.0 of their online themes with numerous changes and enhancements, notably the migration from `.liquid` template files to `.json` template files. This impacted the process of installing Nosto in any of the version 2.0 themes as Nosto mostly expects and modifes `.liquid` files. Nosto has recently released support for version 2.0 themes and this documentation will discuss the details in-depth.
For more information Shopify side of changes, visit corresponding links below,
1. [Online 2.0 themes](https://www.shopify.com/partners/blog/shopify-online-store)
2. [JSON Templates](https://shopify.dev/themes/architecture/templates/json-templates)
3. [Migration Guide](https://shopify.dev/themes/os20)

## Nosto's implementation

After the recent update, Nosto supports for the following,
1. Existing Shopify themes (let's call it version 1.0 themes)
2. Online store 2.0 themes (let's call it version 2.0 themes)
3. Hybrid themes (will discuss more shortly)

### Existing Shopify themes

Support and process for version 1.0 of shopify themes will remain unchanged

### Online 2.0 themes

In order to support the new `JSON` templates and inject Nosto snippet, following changes will be made to the installed theme (For e.g, the [Dawn](https://themes.shopify.com/themes/dawn/styles/default) theme). 

#### Existing JSON templates

> Following sections depict the changes to existing JSON templates while installing Nosto

##### templates/404.json

The following content will be added under `sections` block

```json
"nosto_404": {
      "type": "nosto-404",
      "settings": {
      }
    }
```

The section name `nosto_404` from above content will be updated into `order` section to completed the setup. This schema structure is mandatory in order for the theme to work. A complete example is given below,

```json
{
  "sections": {
    "main": {
      "type": "main-404",
      "settings": {
      }
    },
    "nosto_404": {
      "type": "nosto-404",
      "settings": {
      }
    }
  },
  "order": [
    "main",
    "nosto_404"
  ]
}
```

##### templates/cart.json

The following content will be added under `sections` block

```json
"nosto_cart": {
      "type": "nosto-cart",
      "settings": {
      }
    }
```

The section name `nosto_cart` from above content will be updated into `order` section to completed the setup. A complete example is given below,

```json
{
  "sections": {
    "featured-products": {
      "type": "featured-collection",
      "settings": {
        "collection": "all",
        "image_ratio": "square"
      }
    },
    "cart-items": {
      "type": "main-cart-items",
      "settings": {
      }
    },
    "nosto_cart": {
      "type": "nosto-cart",
      "settings": {
      }
    }
 },
 "order": [
    "cart-items",
    "cart-footer",
    "featured-products",
    "nosto_cart"
  ]
}
```

##### templates/collection.json

Content for `collection` differs slightly from the above two files. For collection, Nosto needs some snippet to be added to the top of the page and some to the bottom. Hence, we introducted two section values having `append` a nd `prepend` suffxes accordingly

```json
"nosto_collection_append": {
      "type": "nosto-collection-append",
      "settings": {
      }
    },
    "nosto_collection_prepend": {
      "type": "nosto-collection-prepend",
      "settings": {
      }
    }
```

The section names `nosto_collection_prepend` and `nosto_collection_append` from above content will be updated into `order` section, at the appropriate position, to completed the setup. A complete example is given below,

```json
{
  "sections": {
    "banner": {
      "type": "main-collection-banner",
      "settings": { }
    },
    "product-grid": {
      "type": "main-collection-product-grid",
      "settings": {}
    },
    "nosto_collection_append": {
      "type": "nosto-collection-append",
      "settings": { }
    },
    "nosto_collection_prepend": {
      "type": "nosto-collection-prepend",
      "settings": { }
    }
  },
  "order": [
    "nosto_collection_prepend",
    "banner",
    "product-grid",
    "nosto_collection_append"
  ]
}
```

##### templates/index.json

The following content will be added under `sections` block

```json
"nosto_index": {
      "type": "nosto-index",
      "settings": {
      }
    }
```

The section name `nosto_index` from above content will be updated into `order` section to completed the setup. A complete example is given below,

```json
{
  "sections": {
    "image_banner": {
      "type": "image-banner",
      "settings": { }
    },
    "rich_text": {
      "type": "rich-text",
      "settings": { }
    },
    "nosto_index": {
      "type": "nosto-index",
      "settings": { }
    }
  },
  "order": [
    "nosto_index",
    "image_banner",
    "rich_text"
  ]
}
```

##### templates/product.json

The following content will be added under `sections` block

```json
"nosto_product": {
      "type": "nosto-product",
      "settings": {
      }
    }
```

The section name `nosto_product` from above content will be updated into `order` section to completed the setup. A complete example is given below,

```json
{
  "sections": {
    "main": {
      "type": "main-product",
      "blocks": {
        "vendor": {
          "type": "text",
          "settings": {
            "text": "{{ product.vendor }}",
            "text_style": "uppercase"
          }
        },
        "title": {
          "type": "title",
          "settings": {
          }
        }
      },
      "block_order": [
        "vendor",
        "title"
      ],
      "settings": { }
    },
    "product-recommendations": {
      "type": "product-recommendations",
      "settings": { }
    },
    "nosto_product": {
      "type": "nosto-product",
      "settings": {
      }
    }
  },
  "order": [
    "main",
    "product-recommendations",
    "nosto_product"
  ]
}
```

##### templates/search.json

This is similar to `collection` and requires content to be added at the beginning and end of category page. Hence, we introduce two section values `prepend` and `append` under `sections`,

```json
"nosto_search_prepend": {
      "type": "nosto-search-prepend",
      "settings": {
      }
    },
"nosto_search_append": {
      "type": "nosto-search-append",
      "settings": {
      }
    }
```

The section names `nosto_search_prepend` and `nosto_search_append` from above content will be updated into `order` section, at the appropriate position, to completed the setup. A complete example is given below,

```json
{
  "sections": {
    "main": {
      "type": "main-search",
      "settings": {
      }
    },
    "nosto_search_prepend": {
      "type": "nosto-search-prepend",
      "settings": {
      }
    },
    "nosto_search_append": {
      "type": "nosto-search-append",
      "settings": {
      }
    }
  },
  "order": [
    "nosto_search_prepend",
    "main",
    "nosto_search_append"
  ]
}
```


#### New liquid templates

> Each of the section values/properties introduced in the section above needs the corresponding liquid present under sections group of the theme. Following sections will list out all the new liquid template files that will be introduced along with it's content

##### sections/nosto-404.liquid

```json
{% render 'nosto-element', id:'notfound-nosto-1' %}
{% render 'nosto-element', id:'notfound-nosto-2' %}
{% render 'nosto-element', id:'notfound-nosto-3' %}
```

##### sections/nosto-cart.liquid

```json
{% render 'nosto-element', id:'cartpage-nosto-1' %}
{% render 'nosto-element', id:'cartpage-nosto-2' %}
{% render 'nosto-element', id:'cartpage-nosto-3' %}
```

##### sections/nosto-collection-prepend.liquid

```json
{% render 'nosto-element', id:'categorypage-nosto-1' %}
```

##### sections/nosto-collection-append.liquid

```json
{% render 'nosto-element', id:'categorypage-nosto-2' %}
```

##### sections/nosto-index.liquid

```json
{% render 'nosto-element', id:'frontpage-nosto-1' %}
{% render 'nosto-element', id:'frontpage-nosto-2' %}
{% render 'nosto-element', id:'frontpage-nosto-3' %}
{% render 'nosto-element', id:'frontpage-nosto-4' %}
```

##### sections/nosto-product.liquid

```json
{% render 'nosto-element', id:'productpage-nosto-1' %}
{% render 'nosto-element', id:'productpage-nosto-2' %}
{% render 'nosto-element', id:'productpage-nosto-3' %}
```

##### sections/nosto-search-prepend.liquid

```json
{% render 'nosto-element', id:'searchpage-nosto-1' %}
```

##### sections/nosto-search-append.liquid

```json
{% render 'nosto-element', id:'searchpage-nosto-2' %}
```

