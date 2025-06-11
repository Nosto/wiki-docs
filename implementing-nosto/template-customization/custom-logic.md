# Custom logic

If you want to attach stateful logic as event handlers to your template elements, [Petite Vue](https://github.com/vuejs/petite-vue) is a useful tool. petite-vue is an alternative distribution of Vue optimized for progressive enhancement. It provides the same template syntax and reactivity mental model as standard Vue. However, it is specifically optimized for "sprinkling" a small amount of interactions on an existing HTML page rendered by a server framework.

Some use cases where Petite Vue is useful will be listed below

## Event handlers to call external APIs and libraries

This example is stateless and show cases how functions can be exposed to template

```markup
<script type="module">
import { createApp } from "https://unpkg.com/petite-vue?module"

createApp({ 
  addToCart(productId) {
    // call platform specific add to cart API  
  }    
 }).mount("#$divId")
</script>
```

Usage from template

```
<span @click="addToCart('$product.productId')">Add to cart</span>
```

## Product selection for bundle creation and related total

In this case the selection state is kept in Petite vue and hooked into the template

```markup
<script type="module">
import { createApp } from "https://unpkg.com/petite-vue?module"

const prices = {
#foreach($product in $!products)
  "$product.productId": $product.price.asNumber(),
#end
}

createApp({  
  selected: [],
  toggle(id) {
    const idx = this.selected.indexOf(id)
    if (idx > -1) {
      this.selected.splice(idx, 1)
    } else {
      this.selected.push(id)
    }
  },
  get total() {
    return this.selected
      .map(id => prices[id] ?? 0)
      .reduce((acc, curr) => acc + curr, 0)
  }
}).mount("#$divId")
</script>
```

template usage

```
<div class="product-grid">
#foreach($product in $!products)
  <div class="product" @click="toggle("$product.productId")">
    ...
  </div>
#end
</div>

<div>Total: {{ total }}
```
