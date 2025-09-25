# Preact Inject

The inject module provides dynamic component injection and rendering capabilities, allowing you to seamlessly integrate search components into existing websites and applications without major architectural changes. It's designed for progressive enhancement and easy integration.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { injectSearchComponents } from '@nosto/search-js/preact/inject';

// Inject components into existing DOM elements
injectSearchComponents({
  searchInput: '#search-box',
  searchResults: '#search-results',
  filters: '.search-filters',
  pagination: '.pagination'
}, {
  accountId: 'your-account-id'
});
```

## Core Features

### Component Injection

Inject search components into existing DOM elements.

```tsx
import { 
  injectComponent,
  injectSearchInput,
  injectSearchResults,
  injectFilters
} from '@nosto/search-js/preact/inject';

// Basic component injection
injectComponent(
  'SearchInput',
  document.getElementById('search-container'),
  {
    placeholder: 'Search products...',
    onSearch: (query) => performSearch(query)
  }
);

// Specialized injections
injectSearchInput('#search-input', {
  autocomplete: true,
  voice: true,
  placeholder: 'What are you looking for?'
});

injectSearchResults('.results-container', {
  view: 'grid',
  columns: 4,
  showPagination: true
});

injectFilters('.sidebar-filters', {
  collapsible: true,
  showClearAll: true
});
```

### Progressive Enhancement

Enhance existing HTML with search functionality.

```html
<!-- Existing HTML -->
<div id="search-form">
  <input type="text" id="search-input" placeholder="Search...">
  <button id="search-button">Search</button>
</div>

<div id="product-grid">
  <div class="product-card">...</div>
  <div class="product-card">...</div>
</div>
```

```tsx
import { enhanceExistingElements } from '@nosto/search-js/preact/inject';

// Enhance existing elements
enhanceExistingElements({
  searchInput: {
    selector: '#search-input',
    enhance: (element) => ({
      autocomplete: true,
      debounce: 300,
      onSearch: (query) => updateResults(query)
    })
  },
  
  searchButton: {
    selector: '#search-button',
    enhance: (element) => ({
      onClick: () => performSearch(getCurrentQuery())
    })
  },
  
  productGrid: {
    selector: '#product-grid',
    enhance: (element) => ({
      lazyLoad: true,
      infiniteScroll: true,
      onProductClick: (product) => trackClick(product)
    })
  }
});
```

### Dynamic Loading

Load and inject components dynamically based on user interaction.

```tsx
import { 
  loadComponent,
  injectOnDemand,
  preloadComponents 
} from '@nosto/search-js/preact/inject';

// Load component on demand
const loadSearchOnFocus = async () => {
  const SearchComponent = await loadComponent('AutocompleteInput');
  
  injectComponent(
    SearchComponent,
    document.getElementById('search-container'),
    {
      onFocus: () => console.log('Search focused'),
      onBlur: () => console.log('Search blurred')
    }
  );
};

// Inject based on user interaction
injectOnDemand({
  trigger: 'click',
  selector: '#search-trigger',
  component: 'SearchModal',
  target: '#modal-container',
  props: {
    visible: true,
    onClose: () => removeComponent('#modal-container')
  }
});

// Preload components for better performance
preloadComponents([
  'SearchInput',
  'SearchResults',
  'ProductCard'
]);
```

## Integration Patterns

### E-commerce Platform Integration

Integration with popular e-commerce platforms.

```tsx
import { integrateWithShopify } from '@nosto/search-js/preact/inject';

// Shopify integration
integrateWithShopify({
  searchSelector: '[data-search-input]',
  resultsSelector: '[data-search-results]',
  collectionSelector: '[data-collection-products]',
  productSelector: '[data-product-card]',
  
  config: {
    accountId: 'your-nosto-account',
    shopifyDomain: 'your-shop.myshopify.com',
    currency: 'USD'
  },
  
  // Custom renderers
  renderProduct: (product) => `
    <div class="product-item">
      <img src="${product.imageUrl}" alt="${product.name}">
      <h3>${product.name}</h3>
      <p>${product.formattedPrice}</p>
    </div>
  `,
  
  // Event handlers
  onProductClick: (product) => {
    gtag('event', 'select_item', {
      item_id: product.id,
      item_name: product.name,
      item_category: product.category
    });
  }
});
```

### WordPress Integration

```tsx
import { integrateWithWordPress } from '@nosto/search-js/preact/inject';

// WordPress WooCommerce integration
integrateWithWordPress({
  searchForm: '.search-form',
  searchResults: '.search-results',
  productArchive: '.woocommerce-products',
  
  config: {
    accountId: 'your-nosto-account',
    wordPressApiUrl: '/wp-json/nosto/v1',
    enableAjax: true
  },
  
  // Override WordPress search
  replaceDefaultSearch: true,
  
  // Custom styling
  cssFramework: 'bootstrap', // 'tailwind', 'bulma', 'custom'
  
  // Integration hooks
  beforeSearch: (query) => {
    // Show loading state
    jQuery('.search-results').addClass('loading');
  },
  
  afterSearch: (results) => {
    // Hide loading state
    jQuery('.search-results').removeClass('loading');
  }
});
```

### Headless CMS Integration

```tsx
import { integrateWithHeadlessCMS } from '@nosto/search-js/preact/inject';

// Integration with Contentful, Strapi, etc.
integrateWithHeadlessCMS({
  cmsType: 'contentful',
  spaceId: 'your-space-id',
  accessToken: 'your-access-token',
  
  // Content mapping
  contentTypes: {
    product: {
      id: 'fields.productId',
      name: 'fields.title',
      description: 'fields.description',
      price: 'fields.price',
      image: 'fields.image.fields.file.url'
    }
  },
  
  // Search endpoint
  searchEndpoint: '/api/search',
  
  // Injection targets
  targets: {
    searchInput: '[data-search]',
    searchResults: '[data-results]',
    filters: '[data-filters]'
  }
});
```

## Legacy Support

### jQuery Integration

Support for jQuery-based websites.

```tsx
import { createJQueryPlugin } from '@nosto/search-js/preact/inject';

// Create jQuery plugin
const nostoSearchPlugin = createJQueryPlugin({
  componentMap: {
    search: 'SearchInput',
    results: 'SearchResults',
    filters: 'FilterSidebar',
    pagination: 'Pagination'
  },
  
  defaultConfig: {
    accountId: 'your-account-id',
    debounce: 300,
    minLength: 2
  }
});

// Register plugin
jQuery.fn.nostoSearch = nostoSearchPlugin;

// Usage
jQuery('#search-container').nostoSearch({
  type: 'search',
  placeholder: 'Search products...',
  onSearch: function(query) {
    console.log('Searching for:', query);
  }
});

jQuery('.results-grid').nostoSearch({
  type: 'results',
  view: 'grid',
  columns: 4
});
```

### Vanilla JavaScript Integration

For sites without frameworks.

```tsx
import { createVanillaIntegration } from '@nosto/search-js/preact/inject';

// Create vanilla JS integration
const NostoSearch = createVanillaIntegration({
  components: ['SearchInput', 'SearchResults', 'FilterSidebar'],
  globalName: 'NostoSearch'
});

// Usage in vanilla JavaScript
window.NostoSearch.init({
  accountId: 'your-account-id',
  
  components: {
    searchInput: {
      selector: '#search-input',
      config: {
        placeholder: 'Search products...',
        autocomplete: true
      }
    },
    
    searchResults: {
      selector: '#search-results',
      config: {
        view: 'grid',
        columns: 4
      }
    }
  },
  
  events: {
    onSearch: (query) => console.log('Search:', query),
    onProductClick: (product) => console.log('Product clicked:', product)
  }
});
```

## Server-Side Rendering

### SSR Component Injection

Inject components that work with server-side rendering.

```tsx
import { 
  injectSSRComponents,
  hydrateComponents 
} from '@nosto/search-js/preact/inject';

// Server-side injection
const serverHTML = injectSSRComponents({
  searchInput: {
    target: '#search-container',
    component: 'SearchInput',
    props: {
      placeholder: 'Search products...',
      initialValue: serverQuery
    }
  },
  
  searchResults: {
    target: '#results-container',
    component: 'SearchResults',
    props: {
      initialResults: serverResults,
      view: 'grid'
    }
  }
});

// Client-side hydration
hydrateComponents({
  '#search-container': {
    component: 'SearchInput',
    props: {
      onSearch: (query) => performClientSearch(query)
    }
  },
  
  '#results-container': {
    component: 'SearchResults',
    props: {
      onProductClick: (product) => trackClick(product)
    }
  }
});
```

## Configuration

### Injection Configuration

```tsx
const injectionConfig = {
  // Global settings
  namespace: 'nosto-search',
  cleanupOnUnmount: true,
  preventStyleConflicts: true,
  
  // Component loading
  lazyLoading: true,
  preloadComponents: ['SearchInput'],
  componentChunkSize: 50000, // bytes
  
  // Error handling
  fallbackComponents: {
    SearchInput: BasicInput,
    SearchResults: BasicResults
  },
  onError: (error, componentName) => {
    console.error(`Failed to load ${componentName}:`, error);
  },
  
  // Performance
  enableVirtualization: true,
  batchDOMUpdates: true,
  debounceRendering: 16, // ms
  
  // Styling
  injectCSS: true,
  cssFramework: 'auto-detect',
  themeAdaptation: true,
  
  // Analytics
  trackInjections: true,
  performanceMetrics: true
};

injectSearchComponents(targets, config, injectionConfig);
```

### Target Configuration

```tsx
const targetConfig = {
  // Search input configuration
  searchInput: {
    selector: '#search-box',
    component: 'AutocompleteInput',
    props: {
      placeholder: 'Search...',
      debounce: 300,
      minLength: 2
    },
    
    // Injection options
    mode: 'replace', // 'append', 'prepend', 'wrap'
    preserveAttributes: ['id', 'class', 'data-*'],
    inheritStyles: true,
    
    // Event handling
    events: {
      onMount: (element) => console.log('Mounted:', element),
      onUnmount: (element) => console.log('Unmounted:', element)
    }
  },
  
  // Search results configuration
  searchResults: {
    selector: '.results-container',
    component: 'SearchResults',
    props: {
      view: 'grid',
      columns: { mobile: 2, tablet: 3, desktop: 4 }
    },
    
    // Conditional rendering
    condition: () => document.querySelector('.results-container'),
    
    // Responsive behavior
    responsive: {
      mobile: { columns: 2, view: 'list' },
      desktop: { columns: 4, view: 'grid' }
    }
  }
};
```

## Error Handling

### Graceful Degradation

```tsx
import { 
  withFallback,
  createErrorBoundary 
} from '@nosto/search-js/preact/inject';

// Component with fallback
const SafeSearchInput = withFallback(
  'SearchInput',
  ({ placeholder }) => (
    <input 
      type="text" 
      placeholder={placeholder}
      onKeyPress={(e) => e.key === 'Enter' && performBasicSearch(e.target.value)}
    />
  )
);

// Error boundary for injected components
const SearchErrorBoundary = createErrorBoundary({
  fallback: (error) => (
    <div className="search-error">
      <p>Search is temporarily unavailable</p>
      <button onClick={() => window.location.reload()}>
        Refresh Page
      </button>
    </div>
  ),
  
  onError: (error, componentName) => {
    console.error('Search component error:', error);
    trackError('component_injection_error', {
      component: componentName,
      error: error.message
    });
  }
});

// Inject with error boundary
injectComponent(
  SearchErrorBoundary,
  document.getElementById('search-container'),
  { children: SafeSearchInput }
);
```

## Testing

### Injection Testing

```tsx
import { 
  mockInjection,
  testComponentInjection 
} from '@nosto/search-js/preact/inject/testing';

test('should inject search input component', async () => {
  const container = document.createElement('div');
  container.id = 'search-container';
  document.body.appendChild(container);

  await injectComponent('SearchInput', container, {
    placeholder: 'Test search...'
  });

  expect(container.querySelector('input')).toHaveAttribute(
    'placeholder', 
    'Test search...'
  );
});

test('should handle injection errors gracefully', async () => {
  const mockError = jest.fn();
  
  await testComponentInjection({
    component: 'NonExistentComponent',
    target: '#test-container',
    onError: mockError
  });

  expect(mockError).toHaveBeenCalled();
});
```

## Performance Optimization

### Code Splitting

```tsx
import { defineAsyncComponent } from '@nosto/search-js/preact/inject';

// Define async components for code splitting
const AsyncSearchInput = defineAsyncComponent(
  () => import('@nosto/search-js/preact/autocomplete').then(m => m.SearchInput),
  {
    loading: () => <div>Loading search...</div>,
    error: () => <div>Failed to load search</div>,
    timeout: 5000
  }
);

// Inject async component
injectComponent(AsyncSearchInput, '#search-container');
```

### Bundle Optimization

```tsx
import { 
  optimizeBundle,
  treeshakeComponents 
} from '@nosto/search-js/preact/inject';

// Tree shake unused components
const optimizedBundle = treeshakeComponents([
  'SearchInput',
  'SearchResults',
  'ProductCard'
]);

// Optimize bundle for injection
const injectionBundle = optimizeBundle({
  components: optimizedBundle,
  minify: true,
  gzip: true,
  splitting: true
});

// Use optimized bundle
injectFromBundle(injectionBundle, targetConfig);
```

## Best Practices

1. **Progressive Enhancement**: Start with basic HTML and enhance with search components
2. **Error Handling**: Always provide fallbacks for failed injections
3. **Performance**: Use lazy loading and code splitting for better performance
4. **Testing**: Test injection in various environments and browsers
5. **Cleanup**: Properly cleanup injected components when no longer needed
6. **Styling**: Ensure injected components adapt to existing site styles
7. **Accessibility**: Maintain accessibility standards in injected components