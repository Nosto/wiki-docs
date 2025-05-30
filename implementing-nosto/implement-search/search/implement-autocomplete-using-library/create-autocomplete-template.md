# Create Autocomplete template

### Requirements

The library handles events through `dataset` properties to avoid handling logic in a template. These `data-*` attributes are used:

1. `data-ns-hit` - this attribute should be used on clickable `keyword`, `product`, `history` list elements. Stringified unmodified JSON object (_**product**_, _**keyword**_ or _**history**_ hit) from the search response should be provided as a value. You will need to escape it in Liquid and Mustache templates.\
   This attribute handles submit keyword/history as search, redirect to product, analytics (if enabled) request.
2. `data-ns-remove-history` - should be used to delete history entries in the autocomplete.\


* To make an element delete a single history entry when clicked, add `data-ns-remove-history={hit.item}` to an element.
* To delete all history entries, add `data-ns-remove-history="all"` to clear button.

### Template reference

This section provides links to default startup templates for different rendering frameworks. These templates can be copied and customized as needed.&#x20;

[Handlebars](https://github.com/Nosto/nosto-autocomplete/blob/main/src/handlebars/autocomplete.handlebars), [Mustache](https://github.com/Nosto/nosto-autocomplete/blob/main/src/mustache/autocomplete.mustache), [Liquid](https://github.com/Nosto/nosto-autocomplete/blob/main/src/liquid/autocomplete.liquid), [React/Preact (HTML)](https://github.com/Nosto/nosto-autocomplete/blob/main/src/react/Autocomplete.tsx)

### Additional information

_**Mustache is based on logic-less templates which can be enhanced with helpers, e.g `toJson`, `imagePlaceholder`, `showListPrice` in example template**_.

```js
import { fromMustacheTemplate } from '@nosto/autocomplete/mustache'

fromMustacheTemplate(template, {
    helpers: {
        toJson: function () {
            return JSON.stringify(this)
        },
    },
})
```
