# NostoSection

The `NostoSection` is a custom element that renders personalized section content using Nosto recommendations. It fetches product recommendations from Nosto and renders them using your site's existing section markup.

## Overview

The NostoSection component combines Nosto's personalization engine with your site's native section rendering. It takes a placement ID to fetch recommendations and a section ID to determine how those products should be displayed using your existing section templates.

## Attributes

### `placement`
- **Type**: `string`
- **Required**: Yes
- **Description**: The Nosto placement identifier used to fetch personalized recommendations.

### `section`
- **Type**: `string`  
- **Required**: Yes
- **Description**: The section identifier that determines which section template to use for rendering the recommended products.

## Usage

### Basic Example

```html
<nosto-section 
  placement="frontpage-featured" 
  section="featured-products">
</nosto-section>
```

### Loading State

The component automatically manages loading states. While fetching and rendering content, it will have a `loading` attribute:

```html
<!-- During loading -->
<nosto-section loading placement="..." section="...">
</nosto-section>

<!-- After loading completes -->
<nosto-section placement="..." section="...">
  <!-- Rendered section content -->
</nosto-section>
```

You can style the loading state using CSS:

```css
nosto-section[loading] {
  opacity: 0.5;
  pointer-events: none;
}

nosto-section[loading]::after {
  content: "Loading...";
  display: block;
  text-align: center;
  padding: 20px;
}
```

## How It Works

1. **Fetch Recommendations**: The component uses the `placement` attribute to request personalized product recommendations from Nosto.

2. **Render Section**: It takes the recommended product handles and makes a request to `/search?section_id={section}&q={handles}` to get the rendered section HTML.

3. **Update Content**: The returned section HTML is parsed and inserted into the component, with any heading updated to match the recommendation title if provided.

4. **Enable Tracking**: Product click tracking is automatically enabled for the rendered content.

## Integration Requirements

### Section Endpoint

Your application must provide a `/search` endpoint that accepts:
- `section_id`: The section template to use
- `q`: Colon-separated list of product handles

The endpoint should return HTML containing the rendered section with the specified products.

### Nosto Setup

The component requires:
- Nosto JavaScript library to be loaded
- Valid Nosto account configuration
- Placement configuration in Nosto admin

## Error Handling

If the section fetch fails (e.g., invalid section ID or server error), the component will throw an error:

```javascript
// This will throw: "Failed to fetch section {section-id}"
```

You can handle this by wrapping the component in error boundaries or listening for errors:

```javascript
document.addEventListener('error', (event) => {
  if (event.target.tagName === 'NOSTO-SECTION') {
    console.error('NostoSection failed to load:', event.error);
    // Handle error appropriately
  }
});
```

## Styling

The component renders your existing section markup, so all your existing section styles will apply. The component itself is a container element that you can style as needed:

```css
nosto-section {
  display: block;
  margin: 20px 0;
}
```

## Browser Support

NostoSection uses modern web APIs including:
- Custom Elements
- Fetch API
- DOMParser
- URL constructor

Ensure these are available in your target browsers or include appropriate polyfills.

## Related Components

This component extends `NostoElement` and works alongside other Nosto web components. Documentation for additional components will be added as they become available.