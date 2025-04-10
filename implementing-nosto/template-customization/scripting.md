# Scripting

{% hint style="info" %}
The content of this page only applies to templates used within:
* Product Recommendations
* Onsite Content Personalization
* Pop-Ups
{% endhint %}

Nosto campaign templates support two ways to define Javascript script elements as part of the templates. 

In the legacy mode the script contents are evaluated in the scope of the Nosto iframe and can refer to the main window via the global `_targetWindow` variable.

 To support ES module loading in placement and popup script elements the client script supports also the usage of `script[type='module']` elements in both of these contexts. This newer module mode is evaluated in the main window, but uses module scope for sandboxing. To write variables to the global scope you will need to do so explicitly by declaring fields in the window object.

For new accounts we recommend the use of ES module scripts and for older accounts with existing templates the legacy script mode works as well, but interaction with the main window is a bit more verbose.

The differences between the two modes are summarized here:

Legacy scripts
* Syntax: `<script>...</script>`
* Loaded into the Nosto iframe sandbox
* Access to site window happens via the `_targetWindow` variable
* Helper modules in the nosto window namespace are available without a prefix

Module scripts
* Syntax: `<script type="module">...</script>`
* Loaded as sandboxed modules into the site window
* site window contents are directly available, e.g. jQuery
* Helper modules in the nosto window namespace are available via the nosto. prefix

## Examples

```markup
<script>
  _targetWindow.jQuery(…
</script>
```

becomes

```markup
<script type=”module”>
  jQuery(…
</script>
```

Additionally module scripts support

import syntax

```markup
<script type="module">
  import { createApp } from 'https://unpkg.com/petite-vue?module'
  createApp().mount()
</script>
```

top level await

```markup
<script type="module">
  const response = await fetch('http://www.acme.com/myapi/myresource')
  // do something with the response
</script>
```

lightweight sandboxing

```markup
<script type="module">
  // config is not written to the window namespace, but scoped to the script tag
  const config = {
    key: "738209438"
  }
</script>
```

## Further reading

[JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)