# Installation

You can install the `Nosto Autocomplete` library via npm:

```bash
npm install @nosto/autocomplete
```

The Nosto Autocomplete library can be imported and used in various ways, depending on your preferred framework or template language. Some of the supported import methods include:

<table><thead><tr><th width="159">Framework</th><th>Import Statement</th></tr></thead><tbody><tr><td>Base</td><td><code>import { autocomplete } from "@nosto/autocomplete"</code></td></tr><tr><td>Mustache</td><td><code>import { autocomplete, fromMustacheTemplate, defaultMustacheTemplate } from "@nosto/autocomplete/mustache"</code></td></tr><tr><td>Liquid</td><td><code>import { autocomplete, fromLiquidTemplate, defaultLiquidTemplate } from "@nosto/autocomplete/liquid"</code></td></tr><tr><td>Preact</td><td><code>import { autocomplete, Autocomplete } from "@nosto/autocomplete/preact"</code></td></tr><tr><td>React</td><td><code>import { autocomplete, Autocomplete } from "@nosto/autocomplete/react"</code></td></tr></tbody></table>

Choose the import method that aligns with your project's requirements and technology stack.

❗Do not combine multiple imports as it will fetch multiple bundles.❗
