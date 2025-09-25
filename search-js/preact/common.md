# Preact Common Components

The common module provides shared components, utilities, and configuration that are used across all other Preact modules. It includes providers, context management, base components, and common UI elements that form the foundation of the search interface.

## Installation

```bash
npm install @nosto/search-js
```

## Core Components

### SearchProvider

Main context provider that manages global search state and configuration.

```tsx
import { SearchProvider } from '@nosto/search-js/preact/common';

const App = () => (
  <SearchProvider
    config={{
      accountId: 'your-account-id',
      apiUrl: 'https://api.nosto.com/search',
      apiKey: 'your-api-key',
      currency: 'USD',
      language: 'en',
      debounce: 300,
      pageSize: 20,
      enableAnalytics: true,
      enablePersonalization: true
    }}
    decorators={[
      priceDecorator({ currency: 'USD' }),
      thumbnailDecorator({ width: 300, height: 300 })
    ]}
    onError={(error) => console.error('Search error:', error)}
  >
    <SearchApp />
  </SearchProvider>
);
```

### SearchContext

Hook for accessing search context and state.

```tsx
import { useSearchContext } from '@nosto/search-js/preact/common';

const SearchComponent = () => {
  const {
    // Configuration
    config,
    updateConfig,
    
    // Search state
    query,
    results,
    loading,
    error,
    total,
    facets,
    
    // Search actions
    search,
    clearSearch,
    setQuery,
    
    // Pagination state
    currentPage,
    pageSize,
    totalPages,
    
    // Pagination actions
    goToPage,
    nextPage,
    previousPage,
    setPageSize,
    
    // Filter state
    activeFilters,
    
    // Filter actions
    addFilter,
    removeFilter,
    clearFilters,
    
    // Analytics
    trackEvent
  } = useSearchContext();

  return (
    <div>
      <input 
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        onKeyPress={(e) => e.key === 'Enter' && search(query)}
      />
      {/* Rest of component */}
    </div>
  );
};
```

## Base Components

### Button

Configurable button component with various styles and states.

```tsx
import { Button } from '@nosto/search-js/preact/common';

<Button
  variant="primary" // "secondary", "outline", "ghost", "link"
  size="medium" // "small", "large"
  disabled={false}
  loading={false}
  fullWidth={false}
  icon={<SearchIcon />}
  iconPosition="left" // "right"
  onClick={() => handleClick()}
  className="custom-button"
>
  Search Products
</Button>
```

### Input

Enhanced input component with built-in validation and styling.

```tsx
import { Input } from '@nosto/search-js/preact/common';

<Input
  type="text"
  placeholder="Search..."
  value={value}
  onChange={(e) => setValue(e.target.value)}
  error={error}
  disabled={false}
  loading={false}
  icon={<SearchIcon />}
  iconPosition="left"
  clearable={true}
  onClear={() => setValue('')}
  debounce={300}
  onDebouncedChange={(value) => performSearch(value)}
  validation={{
    required: true,
    minLength: 2,
    pattern: /^[a-zA-Z0-9\s]+$/
  }}
/>
```

### Modal

Accessible modal component with overlay and focus management.

```tsx
import { Modal } from '@nosto/search-js/preact/common';

<Modal
  visible={modalVisible}
  title="Filter Products"
  size="medium" // "small", "large", "fullscreen"
  closable={true}
  maskClosable={true}
  keyboard={true}
  centered={true}
  onClose={() => setModalVisible(false)}
  onAfterOpen={() => console.log('Modal opened')}
  onAfterClose={() => console.log('Modal closed')}
  footer={
    <div>
      <Button onClick={() => setModalVisible(false)}>Cancel</Button>
      <Button variant="primary" onClick={applyFilters}>Apply</Button>
    </div>
  }
>
  <FilterContent />
</Modal>
```

### Dropdown

Dropdown component with keyboard navigation and accessibility.

```tsx
import { Dropdown } from '@nosto/search-js/preact/common';

<Dropdown
  trigger={<Button>Sort By</Button>}
  placement="bottomLeft" // "bottomRight", "topLeft", "topRight"
  arrow={true}
  disabled={false}
  onVisibleChange={(visible) => setDropdownVisible(visible)}
>
  <Dropdown.Item onClick={() => setSortBy('relevance')}>
    Most Relevant
  </Dropdown.Item>
  <Dropdown.Item onClick={() => setSortBy('price_asc')}>
    Price: Low to High
  </Dropdown.Item>
  <Dropdown.Item onClick={() => setSortBy('price_desc')}>
    Price: High to Low
  </Dropdown.Item>
  <Dropdown.Divider />
  <Dropdown.Item onClick={() => setSortBy('rating')}>
    Highest Rated
  </Dropdown.Item>
</Dropdown>
```

### Badge

Small badge component for counts and labels.

```tsx
import { Badge } from '@nosto/search-js/preact/common';

<Badge
  count={5}
  showZero={false}
  overflowCount={99}
  dot={false}
  status="default" // "success", "processing", "error", "warning"
  color="#1890ff"
  offset={[10, 10]}
>
  <FilterIcon />
</Badge>
```

## Layout Components

### Grid

Responsive grid system for layout management.

```tsx
import { Grid } from '@nosto/search-js/preact/common';

<Grid
  columns={4}
  gap="16px"
  responsive={{
    mobile: 2,
    tablet: 3,
    desktop: 4
  }}
  align="stretch"
  justify="start"
>
  {products.map(product => (
    <Grid.Item key={product.id}>
      <ProductCard product={product} />
    </Grid.Item>
  ))}
</Grid>
```

### Container

Responsive container with max-width constraints.

```tsx
import { Container } from '@nosto/search-js/preact/common';

<Container
  maxWidth="1200px"
  padding="16px"
  centered={true}
  fluid={false}
  className="search-container"
>
  <SearchInterface />
</Container>
```

### Flex

Flexbox layout component.

```tsx
import { Flex } from '@nosto/search-js/preact/common';

<Flex
  direction="row" // "column"
  align="center" // "start", "end", "stretch"
  justify="space-between" // "start", "end", "center"
  wrap="nowrap" // "wrap"
  gap="16px"
>
  <Flex.Item flex={1}>
    <SearchInput />
  </Flex.Item>
  <Flex.Item>
    <FilterButton />
  </Flex.Item>
</Flex>
```

## Feedback Components

### Loading

Loading indicators and spinners.

```tsx
import { Loading } from '@nosto/search-js/preact/common';

<Loading
  spinning={true}
  size="default" // "small", "large"
  tip="Loading products..."
  delay={200}
  indicator={<CustomSpinner />}
>
  <SearchResults />
</Loading>
```

### Skeleton

Skeleton loading components for better perceived performance.

```tsx
import { Skeleton } from '@nosto/search-js/preact/common';

<Skeleton
  loading={true}
  active={true}
  paragraph={{ rows: 3 }}
  title={true}
  avatar={true}
>
  <ProductCard product={product} />
</Skeleton>

// Pre-built skeletons
<Skeleton.ProductCard />
<Skeleton.SearchResult />
<Skeleton.FilterGroup />
```

### Empty

Empty state component for no results.

```tsx
import { Empty } from '@nosto/search-js/preact/common';

<Empty
  image={<NoResultsIllustration />}
  imageStyle={{ height: 120 }}
  description="No products found matching your search"
  children={
    <div>
      <Button onClick={clearFilters}>Clear Filters</Button>
      <Button variant="link" onClick={showPopularProducts}>
        Browse Popular Products
      </Button>
    </div>
  }
/>
```

### Message

Toast messages and notifications.

```tsx
import { Message } from '@nosto/search-js/preact/common';

// Programmatic usage
Message.success('Product added to cart');
Message.error('Failed to load products');
Message.warning('Some filters may not be available');
Message.info('Showing results for "shoes"');

// Component usage
<Message
  type="success"
  message="Search completed"
  description="Found 1,247 matching products"
  showIcon={true}
  closable={true}
  duration={4000}
  onClose={() => console.log('Message closed')}
/>
```

## Utility Components

### Portal

Render components outside the normal component tree.

```tsx
import { Portal } from '@nosto/search-js/preact/common';

<Portal target={document.body}>
  <Modal visible={true}>
    <p>This modal is rendered at the body level</p>
  </Modal>
</Portal>
```

### ErrorBoundary

Error boundary for handling component errors.

```tsx
import { ErrorBoundary } from '@nosto/search-js/preact/common';

<ErrorBoundary
  fallback={(error, errorInfo) => (
    <div className="error-boundary">
      <h2>Something went wrong</h2>
      <details>
        <summary>Error details</summary>
        <pre>{error.message}</pre>
      </details>
      <Button onClick={() => window.location.reload()}>
        Reload Page
      </Button>
    </div>
  )}
  onError={(error, errorInfo) => {
    console.error('Component error:', error);
    trackError(error, errorInfo);
  }}
>
  <SearchInterface />
</ErrorBoundary>
```

### ClickOutside

Handle clicks outside of components.

```tsx
import { ClickOutside } from '@nosto/search-js/preact/common';

<ClickOutside onClickOutside={() => setDropdownVisible(false)}>
  <div className="dropdown">
    <DropdownContent />
  </div>
</ClickOutside>
```

## Theme System

### ThemeProvider

Provide theme configuration to all components.

```tsx
import { ThemeProvider } from '@nosto/search-js/preact/common';

const theme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    success: '#28a745',
    danger: '#dc3545',
    warning: '#ffc107',
    info: '#17a2b8',
    light: '#f8f9fa',
    dark: '#343a40'
  },
  typography: {
    fontFamily: '"Helvetica Neue", Arial, sans-serif',
    fontSize: {
      xs: '12px',
      sm: '14px',
      md: '16px',
      lg: '18px',
      xl: '20px'
    },
    fontWeight: {
      light: 300,
      normal: 400,
      medium: 500,
      bold: 700
    }
  },
  spacing: {
    xs: '4px',
    sm: '8px',
    md: '16px',
    lg: '24px',
    xl: '32px',
    xxl: '48px'
  },
  breakpoints: {
    mobile: '576px',
    tablet: '768px',
    desktop: '992px',
    wide: '1200px'
  },
  borderRadius: '4px',
  boxShadow: {
    sm: '0 1px 3px rgba(0,0,0,0.12)',
    md: '0 4px 6px rgba(0,0,0,0.16)',
    lg: '0 10px 25px rgba(0,0,0,0.19)'
  }
};

<ThemeProvider theme={theme}>
  <App />
</ThemeProvider>
```

### useTheme

Hook for accessing theme values.

```tsx
import { useTheme } from '@nosto/search-js/preact/common';

const CustomComponent = () => {
  const theme = useTheme();
  
  return (
    <div 
      style={{
        backgroundColor: theme.colors.primary,
        padding: theme.spacing.md,
        borderRadius: theme.borderRadius
      }}
    >
      Themed component
    </div>
  );
};
```

## Configuration

### Config Management

```tsx
import { ConfigProvider, useConfig } from '@nosto/search-js/preact/common';

const config = {
  search: {
    debounce: 300,
    minLength: 2,
    maxResults: 100
  },
  ui: {
    showImages: true,
    showPrices: true,
    gridColumns: 4
  },
  analytics: {
    trackClicks: true,
    trackViews: true,
    provider: 'nosto'
  }
};

<ConfigProvider config={config}>
  <SearchApp />
</ConfigProvider>

// Access config in components
const MyComponent = () => {
  const { config, updateConfig } = useConfig();
  
  return (
    <div>
      <input
        onChange={(e) => updateConfig({
          search: { minLength: parseInt(e.target.value) }
        })}
      />
    </div>
  );
};
```

## Styling and CSS

### CSS-in-JS Support

```tsx
import { styled } from '@nosto/search-js/preact/common';

const StyledButton = styled(Button)`
  background: linear-gradient(45deg, #007bff, #0056b3);
  border: none;
  color: white;
  font-weight: 600;
  
  &:hover {
    background: linear-gradient(45deg, #0056b3, #004085);
  }
  
  &.disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
`;
```

### CSS Modules

```tsx
import styles from './SearchComponent.module.css';
import { classNames } from '@nosto/search-js/preact/common';

const SearchComponent = ({ variant, size, active }) => {
  return (
    <div 
      className={classNames(
        styles.searchComponent,
        styles[variant],
        styles[size],
        {
          [styles.active]: active,
          [styles.disabled]: !active
        }
      )}
    >
      {/* Component content */}
    </div>
  );
};
```

## Accessibility Utilities

### ARIA Helpers

```tsx
import { 
  useAriaLive, 
  useAriaLabel, 
  useFocusManagement 
} from '@nosto/search-js/preact/common';

const AccessibleComponent = () => {
  const announceMessage = useAriaLive();
  const labelId = useAriaLabel('Search results');
  const { focusRef, trapFocus } = useFocusManagement();

  const handleSearch = () => {
    announceMessage(`Found ${results.length} products`);
  };

  return (
    <div
      ref={focusRef}
      aria-labelledby={labelId}
      onFocus={trapFocus}
    >
      {/* Component content */}
    </div>
  );
};
```

## Performance Utilities

### Memoization Helpers

```tsx
import { 
  memo, 
  useMemoizedCallback,
  useDeepMemo 
} from '@nosto/search-js/preact/common';

const ExpensiveComponent = memo(({ data, onUpdate }) => {
  const processedData = useDeepMemo(() => {
    return data.map(item => processItem(item));
  }, [data]);

  const handleUpdate = useMemoizedCallback((id, updates) => {
    onUpdate(id, updates);
  }, [onUpdate]);

  return (
    <div>
      {processedData.map(item => (
        <Item 
          key={item.id} 
          data={item}
          onUpdate={handleUpdate}
        />
      ))}
    </div>
  );
});
```

## Testing Utilities

### Test Helpers

```tsx
import { 
  renderWithProvider,
  mockSearchProvider,
  waitForSearch 
} from '@nosto/search-js/preact/common/testing';

test('should render search results', async () => {
  const { getByText, getByRole } = renderWithProvider(
    <SearchComponent />,
    {
      initialState: {
        query: 'test',
        results: mockProducts
      }
    }
  );

  expect(getByText('Search Results')).toBeInTheDocument();
  
  await waitForSearch();
  
  expect(getByRole('grid')).toBeInTheDocument();
});
```