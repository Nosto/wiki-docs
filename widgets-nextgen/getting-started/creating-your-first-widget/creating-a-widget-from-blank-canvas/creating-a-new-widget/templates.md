# Templates

#### Templates

***

Template customisations can be made to individual web components, to ensure they generate UI according to your needs.

By default, each web component has its own layout, which renders when that web component is loaded into the screen.

Currently, we have a few web components which can be customised.

```json
  "expanded-tile",
  "products",
  "shopspots",
  "add-to-cart",
  "load-more",
  "tile-content",
  "timephrase",
  "share-menu",
  "tags"
```

These components can be located in the DOM.

In order to add customisations, one can copy of one of the sample templates and begin making changes to the JSX as required.

For example, to modify the load-more web component, we may do something like the following:

**1) Create a template**

```
import { createElement } from "@stackla/widget-utils"

export default function LoadMoreTemplate() {
  return (
    <div id="buttons">
      <a id="load-more">I have changed the load more button to look like this!</a>
    </div>
  )
}
```

**2) Utilise load widget with the customisation in place**

```
import { loadWidget } from "@stackla/widget-utils"
import { LoadMoreTemplate } from "./load-more.template"

loadWidget({
  templates: {
    "load-more": {
      template: LoadMoreTemplate
    }
  }
})
```

**3) Enjoy your new load more button!**
