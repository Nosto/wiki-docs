# Preact Components

The Preact components provide a comprehensive set of modern, reactive UI components for building search interfaces. Built with Preact/React, these components offer high performance, accessibility, and customizable styling while maintaining a small bundle size.

## Overview

The Preact module is organized into several specialized packages:

- **[Autocomplete](autocomplete.md)**: Smart search suggestions and autocomplete functionality
- **[Hooks](hooks.md)**: React hooks for search state management and data fetching
- **[SERP](serp.md)**: Search Engine Results Page components with pagination and sorting
- **[Common](common.md)**: Shared components, utilities, and configuration
- **[Category](category.md)**: Category-based search and navigation components
- **[Events](events.md)**: Event handling, analytics tracking, and user interaction
- **[Inject](inject.md)**: Dynamic component injection and rendering
- **[Legacy](legacy.md)**: Backward compatibility components for existing implementations

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { render } from 'preact';
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';
import { SearchProvider } from '@nosto/search-js/preact/common';

const App = () => (
  <SearchProvider config={{ accountId: 'your-account-id' }}>
    <AutocompleteInput
      placeholder="Search products..."
      onSelect={(product) => console.log('Selected:', product)}
    />
  </SearchProvider>
);

render(<App />, document.getElementById('app'));
```

## Key Features

### ⚛️ React/Preact Compatible

All components work seamlessly with both React and Preact, allowing you to choose the best fit for your project.

```tsx
// Works with React
import React from 'react';
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';

// Works with Preact
import { render } from 'preact';
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';
```

### 🎨 Customizable Styling

Components are designed with customization in mind, supporting CSS modules, styled-components, and custom themes.

```tsx
import { SearchResults } from '@nosto/search-js/preact/serp';

<SearchResults
  className="custom-results"
  theme={{
    primaryColor: '#007bff',
    borderRadius: '8px',
    spacing: '16px'
  }}
  renderProduct={(product) => (
    <CustomProductCard product={product} />
  )}
/>
```

### 📱 Mobile-First Design

All components are built with mobile-first responsive design and touch-friendly interactions.

```tsx
import { MobileSearchInput } from '@nosto/search-js/preact/autocomplete';

<MobileSearchInput
  responsive={true}
  touchOptimized={true}
  gestureNavigation={true}
/>
```

### ♿ Accessibility Support

Components follow WCAG 2.1 AA guidelines with built-in keyboard navigation, screen reader support, and ARIA attributes.

```tsx
import { AccessibleAutocomplete } from '@nosto/search-js/preact/autocomplete';

<AccessibleAutocomplete
  ariaLabel="Search for products"
  announceResults={true}
  keyboardNavigation={true}
/>
```

## Architecture

### Provider Pattern

The library uses React's Context API to manage search state and configuration across components.

```tsx
import { SearchProvider, useSearchContext } from '@nosto/search-js/preact/common';

const SearchApp = () => (
  <SearchProvider 
    config={{
      accountId: 'your-account-id',
      apiUrl: 'https://api.nosto.com/search',
      debounce: 300
    }}
  >
    <SearchInterface />
  </SearchProvider>
);

const SearchInterface = () => {
  const { search, results, loading } = useSearchContext();
  
  return (
    <div>
      <SearchInput onSearch={search} />
      {loading ? <LoadingSpinner /> : <SearchResults results={results} />}
    </div>
  );
};
```

### Hook-Based State Management

Components use custom hooks for state management, making them composable and testable.

```tsx
import { useSearch, useFacets, usePagination } from '@nosto/search-js/preact/hooks';

const SearchPage = () => {
  const { search, results, loading } = useSearch();
  const { facets, toggleFacet } = useFacets();
  const { currentPage, totalPages, goToPage } = usePagination();

  return (
    <div>
      <SearchFilters facets={facets} onToggleFacet={toggleFacet} />
      <SearchResults results={results} loading={loading} />
      <Pagination 
        currentPage={currentPage}
        totalPages={totalPages}
        onPageChange={goToPage}
      />
    </div>
  );
};
```

## Component Categories

### Input Components

- **AutocompleteInput**: Smart search input with suggestions
- **SearchBox**: Basic search input with customizable styling
- **VoiceSearchInput**: Voice-enabled search input
- **FilterInput**: Range and text filters for refinement

### Display Components

- **SearchResults**: Grid or list view of search results
- **ProductCard**: Individual product display component
- **FacetList**: Filterable facet navigation
- **BreadcrumbNavigation**: Search path navigation

### Navigation Components

- **Pagination**: Page-based result navigation
- **LoadMore**: Infinite scroll and load more functionality
- **SortSelector**: Result sorting options
- **ViewToggle**: Switch between grid/list views

### Feedback Components

- **LoadingSpinner**: Search loading indicators
- **EmptyState**: No results found messaging
- **ErrorBoundary**: Error handling and recovery
- **ProgressIndicator**: Search progress feedback

## Theming and Customization

### CSS Custom Properties

```css
:root {
  --nosto-search-primary: #007bff;
  --nosto-search-secondary: #6c757d;
  --nosto-search-background: #ffffff;
  --nosto-search-text: #212529;
  --nosto-search-border: #dee2e6;
  --nosto-search-radius: 4px;
  --nosto-search-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
```

### Theme Provider

```tsx
import { ThemeProvider } from '@nosto/search-js/preact/common';

const customTheme = {
  colors: {
    primary: '#ff6b6b',
    secondary: '#4ecdc4',
    background: '#f8f9fa',
    text: '#2c3e50'
  },
  typography: {
    fontFamily: '"Helvetica Neue", Arial, sans-serif',
    fontSize: '14px',
    lineHeight: 1.5
  },
  spacing: {
    xs: '4px',
    sm: '8px',
    md: '16px',
    lg: '24px',
    xl: '32px'
  }
};

<ThemeProvider theme={customTheme}>
  <SearchApp />
</ThemeProvider>
```

### Component Customization

```tsx
import { SearchResults } from '@nosto/search-js/preact/serp';

<SearchResults
  renderProduct={(product, index) => (
    <div className="custom-product-card">
      <img src={product.imageUrl} alt={product.name} />
      <h3>{product.name}</h3>
      <p>{product.formattedPrice}</p>
      <button onClick={() => addToCart(product)}>
        Add to Cart
      </button>
    </div>
  )}
  renderEmpty={() => (
    <div className="custom-empty-state">
      <h2>No products found</h2>
      <p>Try adjusting your search or filters</p>
    </div>
  )}
  renderLoading={() => (
    <div className="custom-loading">
      <div className="spinner" />
      <p>Searching...</p>
    </div>
  )}
/>
```

## Performance Optimization

### Code Splitting

```tsx
import { lazy, Suspense } from 'preact/compat';

const SearchResults = lazy(() => 
  import('@nosto/search-js/preact/serp').then(m => ({ default: m.SearchResults }))
);

const App = () => (
  <Suspense fallback={<div>Loading...</div>}>
    <SearchResults />
  </Suspense>
);
```

### Memoization

```tsx
import { memo } from 'preact/compat';
import { ProductCard } from '@nosto/search-js/preact/common';

const MemoizedProductCard = memo(ProductCard, (prevProps, nextProps) => {
  return prevProps.product.id === nextProps.product.id &&
         prevProps.product.price === nextProps.product.price;
});
```

### Virtual Scrolling

```tsx
import { VirtualizedSearchResults } from '@nosto/search-js/preact/serp';

<VirtualizedSearchResults
  itemHeight={200}
  containerHeight={600}
  renderItem={({ item, index }) => (
    <ProductCard key={item.id} product={item} />
  )}
/>
```

## TypeScript Support

All components are built with TypeScript and provide comprehensive type definitions:

```tsx
import type { 
  SearchConfig,
  SearchResult,
  AutocompleteProps,
  SearchResultsProps 
} from '@nosto/search-js/preact/types';

interface CustomSearchProps extends SearchResultsProps {
  customProp: string;
}

const CustomSearch: React.FC<CustomSearchProps> = ({ customProp, ...props }) => {
  return <SearchResults {...props} />;
};
```

## Testing

Components are designed to be easily testable with popular testing libraries:

```tsx
import { render, screen, fireEvent } from '@testing-library/preact';
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';

test('should trigger search on input', async () => {
  const onSearch = jest.fn();
  
  render(<AutocompleteInput onSearch={onSearch} />);
  
  const input = screen.getByRole('textbox');
  fireEvent.input(input, { target: { value: 'test query' } });
  
  await waitFor(() => {
    expect(onSearch).toHaveBeenCalledWith('test query');
  });
});
```

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- iOS Safari (latest 2 versions)
- Chrome Mobile (latest)

## Getting Started

1. **Install the package**:
   ```bash
   npm install @nosto/search-js
   ```

2. **Set up the provider**:
   ```tsx
   import { SearchProvider } from '@nosto/search-js/preact/common';
   
   <SearchProvider config={{ accountId: 'your-account-id' }}>
     <App />
   </SearchProvider>
   ```

3. **Add components**:
   ```tsx
   import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';
   import { SearchResults } from '@nosto/search-js/preact/serp';
   
   const SearchPage = () => (
     <div>
       <AutocompleteInput placeholder="Search..." />
       <SearchResults />
     </div>
   );
   ```

## Next Steps

- Explore individual component documentation
- Check out the [examples repository](https://github.com/Nosto/search-js-examples)
- Join the [community discussions](https://github.com/Nosto/search-js/discussions)
- Report issues on [GitHub](https://github.com/Nosto/search-js/issues)