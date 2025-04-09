# Styling

{% hint style="info" %}
The content of this page only applies to templates used within:
* Product Recommendations
* Onsite Content Personalization
* Pop-Ups
{% endhint %}

Styling the recommendations is generally quite straightforward. Just add a style block to the template and use CSS to style the recommendation elements as you would style any HTML content.

## Encapsulating styles

While the basic styling of the recommendations works great, it can be problematic if there are multiple recommendation elements on the same page. If styles from different recommendations share same class names, the last element on the page will override the styles of the previous recommendations.

You can use the `$divId` variable to print out the current placement id in the template. Using the variable you can form CSS selectors that start with the ID of the placement, which limits the scope of the CSS to only the current campaign. You can also target styles to the campaign directly by using the ID selector.

## Nested CSS

We recommend the use of Nested CSS for scoped styling of campaign templates due to it's more compact syntax and wide browser support.

As most of the recommendation template styles should be scoped to a specific slot it is common to see scoping structures like this

```markup
#$divId .nosto-block {
  …
}
#$divId .nosto-header {
 …
}
#$divId .nosto-list {
 …
}
```

which can be expressed like this using nested CSS

```markup
#$divId {
  .nosto-block {
    …
  }
  .nosto-header {
    …
  }
  .nosto-list {
    …
  }
}
```

To make CSS Nesting in placement and popup templates also available for older browsers that don’t support this feature the client script provides a polyfill for this. To use the nesting polyfill you will need to provide the attributed nested on a a style element

Example conversion:

```markup
<style nested>
#$divId {
  .wrapper {
    .blue {
      color: blue;
    }
    .red {
      color: red;
    }
  }
}
</style>
```

becomes the following with `divId` as `nosto-product1`

```markup
<style nested data-transpiled="true">
#nosto-product1 .wrapper .blue { color:blue; }
#nosto-product1 .wrapper .red { color:red; }
</style>
```

The transpilation will be applied in debug mode for all browsers and in normal mode for browsers that don’t support CSS Nesting.

## Further reading

[CSS Nesting](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)

[Browser support](https://caniuse.com/css-nesting)