# Product Image Configuration

## Customizing Product Image Aspect Ratio

You can use CSS custom properties (variables) to define the aspect ratio for specific components. This is useful for components like the autocomplete dropdown where you might want a different aspect ratio than the main product grid.

The project defines a CSS custom property `--ns-aspect-ratio` for this purpose.

#### Example: Autocomplete Product Image

The product images within the autocomplete results use this CSS variable.

**File to inspect:** `src/components/Autocomplete/Item/Product.module.css`

```css
/* src/components/Autocomplete/Item/Product.module.css */
.image {
  height: auto;
  aspect-ratio: var(--ns-aspect-ratio);
  object-fit: contain;
  width: 100%;
}
```

You can override the value of `--ns-aspect-ratio` in your theme's CSS file or directly in the component's stylesheet to change the aspect ratio. The value is defined in `src/variable.css`.

**File to edit:** `src/variable.css`

```css
/* src/variable.css */
:root {
  /* ... other variables ... */

  --ns-aspect-ratio: 1; /* Default to square */
}
```

By changing `--ns-aspect-ratio` to `4 / 3`, for instance, any component using this variable will render images with a `4:3` aspect ratio.
