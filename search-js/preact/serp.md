# Preact SERP Components

The SERP (Search Engine Results Page) module provides comprehensive components for displaying search results, including product grids, lists, pagination, sorting, and filtering interfaces. These components are optimized for e-commerce search experiences with responsive design and accessibility features.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { SearchResults, Pagination, SortSelector } from '@nosto/search-js/preact/serp';
import { SearchProvider } from '@nosto/search-js/preact/common';

const SearchPage = () => (
  <SearchProvider config={{ accountId: 'your-account-id' }}>
    <div className="search-page">
      <div className="search-controls">
        <SortSelector />
      </div>
      <SearchResults />
      <Pagination />
    </div>
  </SearchProvider>
);
```

## Core Components

### SearchResults

Main component for displaying search results in grid or list format.

```tsx
import { SearchResults } from '@nosto/search-js/preact/serp';

<SearchResults
  view="grid" // or "list"
  columns={4}
  gap="16px"
  loading={false}
  renderProduct={(product, index) => (
    <ProductCard 
      key={product.id}
      product={product}
      index={index}
      onView={() => trackView(product)}
      onAddToCart={() => addToCart(product)}
    />
  )}
  renderEmpty={() => (
    <EmptyState 
      title="No products found"
      description="Try adjusting your search or filters"
      action={<button onClick={clearFilters}>Clear Filters</button>}
    />
  )}
  renderLoading={() => (
    <div className="loading-grid">
      {Array.from({ length: 12 }).map((_, i) => (
        <ProductSkeleton key={i} />
      ))}
    </div>
  )}
/>
```

### ProductCard

Configurable product display component with various layouts.

```tsx
import { ProductCard } from '@nosto/search-js/preact/serp';

<ProductCard
  product={product}
  layout="standard" // "compact", "detailed", "minimal"
  showImage={true}
  showPrice={true}
  showRating={true}
  showBrand={true}
  showBadges={true}
  imageAspectRatio="1:1"
  hoverEffects={true}
  onClick={(product) => navigate(`/product/${product.id}`)}
  onAddToCart={(product) => addToCart(product)}
  onQuickView={(product) => openQuickView(product)}
  renderCustomContent={(product) => (
    <div className="custom-content">
      <WishlistButton productId={product.id} />
      <CompareButton productId={product.id} />
    </div>
  )}
/>
```

### Pagination

Comprehensive pagination component with various display modes.

```tsx
import { Pagination } from '@nosto/search-js/preact/serp';

<Pagination
  mode="numbers" // "simple", "load-more", "infinite"
  showFirstLast={true}
  showPrevNext={true}
  showPageInfo={true}
  showPageSizeSelector={true}
  maxVisiblePages={5}
  pageSizeOptions={[12, 24, 48, 96]}
  scrollToTop={true}
  onPageChange={(page) => trackPageChange(page)}
  renderPageButton={(page, isActive) => (
    <button className={`page-btn ${isActive ? 'active' : ''}`}>
      {page}
    </button>
  )}
/>
```

### SortSelector

Dropdown component for sorting search results.

```tsx
import { SortSelector } from '@nosto/search-js/preact/serp';

<SortSelector
  options={[
    { value: 'relevance', label: 'Most Relevant', default: true },
    { value: 'price_asc', label: 'Price: Low to High' },
    { value: 'price_desc', label: 'Price: High to Low' },
    { value: 'name_asc', label: 'Name: A to Z' },
    { value: 'rating_desc', label: 'Highest Rated' },
    { value: 'newest', label: 'Newest First' }
  ]}
  showLabel={true}
  onChange={(sortBy) => trackSortChange(sortBy)}
  renderOption={(option) => (
    <div className="sort-option">
      <span>{option.label}</span>
      {option.popular && <span className="badge">Popular</span>}
    </div>
  )}
/>
```

## Advanced Components

### FilterSidebar

Comprehensive filtering interface with collapsible sections.

```tsx
import { FilterSidebar } from '@nosto/search-js/preact/serp';

<FilterSidebar
  collapsible={true}
  showClearAll={true}
  showActiveCount={true}
  stickyHeader={true}
  renderFacetGroup={(facetGroup, facets) => (
    <FacetGroup
      title={facetGroup.name}
      expandable={true}
      defaultExpanded={facetGroup.important}
    >
      {facets.map(facet => (
        <FacetOption
          key={facet.value}
          facet={facet}
          selected={facet.selected}
          onToggle={() => toggleFacet(facetGroup.key, facet.value)}
        />
      ))}
    </FacetGroup>
  )}
  renderPriceRange={(priceRange) => (
    <PriceRangeSlider
      min={priceRange.min}
      max={priceRange.max}
      value={[priceRange.selectedMin, priceRange.selectedMax]}
      onChange={([min, max]) => setPriceRange(min, max)}
    />
  )}
/>
```

### QuickView

Modal component for quick product preview.

```tsx
import { QuickView } from '@nosto/search-js/preact/serp';

<QuickView
  product={selectedProduct}
  visible={quickViewVisible}
  onClose={() => setQuickViewVisible(false)}
  showGallery={true}
  showVariants={true}
  showDescription={true}
  showReviews={true}
  enableZoom={true}
  onAddToCart={(product, variant) => {
    addToCart(product, variant);
    setQuickViewVisible(false);
  }}
  renderCustomActions={(product) => (
    <div className="custom-actions">
      <WishlistButton productId={product.id} />
      <ShareButton product={product} />
    </div>
  )}
/>
```

### SearchSummary

Display search query and result information.

```tsx
import { SearchSummary } from '@nosto/search-js/preact/serp';

<SearchSummary
  query="running shoes"
  total={1247}
  showing={24}
  page={1}
  appliedFilters={[
    { type: 'brand', label: 'Nike', value: 'nike' },
    { type: 'price', label: '$50 - $150', value: '50-150' }
  ]}
  searchTime={156}
  showDidYouMean={true}
  didYouMean="running shoes"
  onClearFilter={(filter) => clearFilter(filter)}
  onClearAllFilters={() => clearAllFilters()}
  renderSuggestion={(suggestion) => (
    <button onClick={() => search(suggestion)}>
      Did you mean: <strong>{suggestion}</strong>?
    </button>
  )}
/>
```

## Layout Components

### SearchGrid

Responsive grid layout for search results.

```tsx
import { SearchGrid } from '@nosto/search-js/preact/serp';

<SearchGrid
  columns={{
    mobile: 2,
    tablet: 3,
    desktop: 4,
    wide: 5
  }}
  gap="16px"
  aspectRatio="4:5"
  items={products}
  renderItem={(product, index) => (
    <ProductCard product={product} index={index} />
  )}
  renderSkeleton={() => <ProductSkeleton />}
  loading={loading}
  skeletonCount={12}
/>
```

### SearchList

List layout for detailed product information.

```tsx
import { SearchList } from '@nosto/search-js/preact/serp';

<SearchList
  items={products}
  dividers={true}
  zebra={false}
  renderItem={(product, index) => (
    <div className="product-list-item">
      <img src={product.imageUrl} alt={product.name} />
      <div className="product-info">
        <h3>{product.name}</h3>
        <p>{product.description}</p>
        <div className="product-meta">
          <span className="price">{product.formattedPrice}</span>
          <span className="rating">⭐ {product.rating}</span>
        </div>
      </div>
      <div className="product-actions">
        <button onClick={() => addToCart(product)}>
          Add to Cart
        </button>
      </div>
    </div>
  )}
/>
```

### ViewToggle

Switch between grid and list views.

```tsx
import { ViewToggle } from '@nosto/search-js/preact/serp';

<ViewToggle
  view={currentView}
  options={[
    { value: 'grid', icon: '⊞', label: 'Grid View' },
    { value: 'list', icon: '☰', label: 'List View' }
  ]}
  onChange={(view) => setCurrentView(view)}
  showLabels={false}
  size="medium"
/>
```

## Responsive Design

### Mobile-First Components

```tsx
import { MobileSearchResults } from '@nosto/search-js/preact/serp';

<MobileSearchResults
  touchOptimized={true}
  swipeNavigation={true}
  pullToRefresh={true}
  infiniteScroll={true}
  stickyFilters={true}
  renderMobileHeader={() => (
    <div className="mobile-header">
      <SearchInput />
      <FilterButton onClick={openFilters} />
    </div>
  )}
  renderFilterModal={() => (
    <FilterModal
      visible={filtersVisible}
      onClose={() => setFiltersVisible(false)}
    />
  )}
/>
```

### Adaptive Layouts

```tsx
import { AdaptiveSearchResults } from '@nosto/search-js/preact/serp';

<AdaptiveSearchResults
  breakpoints={{
    mobile: 768,
    tablet: 1024,
    desktop: 1200
  }}
  layouts={{
    mobile: { columns: 2, view: 'grid', showFilters: false },
    tablet: { columns: 3, view: 'grid', showFilters: true },
    desktop: { columns: 4, view: 'grid', showFilters: true }
  }}
  renderLayout={(layout, products) => (
    <SearchGrid
      columns={layout.columns}
      items={products}
      showFilters={layout.showFilters}
    />
  )}
/>
```

## Performance Features

### Virtual Scrolling

```tsx
import { VirtualizedSearchResults } from '@nosto/search-js/preact/serp';

<VirtualizedSearchResults
  itemHeight={300}
  containerHeight={600}
  buffer={5}
  items={products}
  renderItem={({ item, index, style }) => (
    <div style={style}>
      <ProductCard product={item} index={index} />
    </div>
  )}
  onScroll={(scrollTop, isScrolling) => {
    if (scrollTop > threshold && !isScrolling) {
      loadMoreProducts();
    }
  }}
/>
```

### Lazy Loading

```tsx
import { LazySearchResults } from '@nosto/search-js/preact/serp';

<LazySearchResults
  threshold={200}
  placeholder={<ProductSkeleton />}
  fadeIn={true}
  renderProduct={(product, isVisible) => (
    <ProductCard
      product={product}
      lazy={!isVisible}
      onEnterViewport={() => trackProductView(product)}
    />
  )}
/>
```

### Image Optimization

```tsx
import { OptimizedProductCard } from '@nosto/search-js/preact/serp';

<OptimizedProductCard
  product={product}
  imageOptimization={{
    lazy: true,
    responsive: true,
    format: 'webp',
    quality: 80,
    sizes: {
      mobile: '150w',
      tablet: '200w',
      desktop: '250w'
    }
  }}
  preloadFirstScreen={index < 8}
/>
```

## Customization

### Custom Product Card

```tsx
const CustomProductCard = ({ product, onAddToCart }) => {
  return (
    <div className="custom-product-card">
      <div className="product-image-container">
        <img src={product.imageUrl} alt={product.name} />
        <div className="product-badges">
          {product.onSale && <span className="sale-badge">Sale</span>}
          {product.isNew && <span className="new-badge">New</span>}
        </div>
        <div className="product-actions">
          <button 
            className="quick-view-btn"
            onClick={() => openQuickView(product)}
          >
            Quick View
          </button>
        </div>
      </div>
      
      <div className="product-info">
        <h3 className="product-name">{product.name}</h3>
        <p className="product-brand">{product.brand}</p>
        
        <div className="product-rating">
          <StarRating rating={product.rating} />
          <span>({product.reviewCount})</span>
        </div>
        
        <div className="product-pricing">
          {product.compareAtPrice && (
            <span className="compare-price">
              {product.formattedCompareAtPrice}
            </span>
          )}
          <span className="current-price">
            {product.formattedPrice}
          </span>
        </div>
        
        <div className="product-variants">
          <ColorSwatches 
            colors={product.colors}
            selected={selectedColor}
            onChange={setSelectedColor}
          />
        </div>
        
        <button 
          className="add-to-cart-btn"
          onClick={() => onAddToCart(product)}
        >
          Add to Cart
        </button>
      </div>
    </div>
  );
};
```

### Theme Integration

```tsx
import { ThemeProvider } from '@nosto/search-js/preact/common';

const customTheme = {
  serp: {
    gridGap: '20px',
    cardBorderRadius: '8px',
    cardShadow: '0 4px 12px rgba(0,0,0,0.1)',
    cardHoverShadow: '0 8px 24px rgba(0,0,0,0.15)',
    primaryColor: '#007bff',
    secondaryColor: '#6c757d',
    backgroundColor: '#f8f9fa',
    textColor: '#212529'
  }
};

<ThemeProvider theme={customTheme}>
  <SearchResults />
</ThemeProvider>
```

## API Reference

### SearchResults Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `view` | `'grid' \| 'list'` | 'grid' | Display mode |
| `columns` | `number \| object` | 4 | Grid columns (responsive) |
| `gap` | `string` | '16px' | Gap between items |
| `loading` | `boolean` | false | Loading state |
| `renderProduct` | `function` | - | Custom product renderer |
| `renderEmpty` | `function` | - | Empty state renderer |
| `renderLoading` | `function` | - | Loading state renderer |

### ProductCard Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `product` | `Product` | - | Product data |
| `layout` | `string` | 'standard' | Card layout style |
| `showImage` | `boolean` | true | Show product image |
| `showPrice` | `boolean` | true | Show product price |
| `showRating` | `boolean` | true | Show product rating |
| `imageAspectRatio` | `string` | '1:1' | Image aspect ratio |
| `onClick` | `function` | - | Click handler |

### Pagination Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `mode` | `string` | 'numbers' | Pagination mode |
| `showFirstLast` | `boolean` | true | Show first/last buttons |
| `showPrevNext` | `boolean` | true | Show prev/next buttons |
| `maxVisiblePages` | `number` | 5 | Max visible page numbers |
| `scrollToTop` | `boolean` | true | Scroll to top on page change |

## Accessibility

All SERP components follow WCAG 2.1 AA guidelines:

- **Keyboard Navigation**: Full keyboard support
- **Screen Reader Support**: Proper ARIA labels and descriptions
- **Focus Management**: Logical focus order
- **High Contrast**: Support for high contrast modes
- **Reduced Motion**: Respects motion preferences

```tsx
<SearchResults
  accessibilityProps={{
    ariaLabel: 'Search results',
    role: 'main',
    announceResultCount: true,
    keyboardNavigation: true
  }}
/>
```

## Testing

```tsx
import { render, screen, fireEvent } from '@testing-library/preact';
import { SearchResults } from '@nosto/search-js/preact/serp';

test('should render search results', () => {
  const products = [
    { id: '1', name: 'Product 1', price: 99.99 }
  ];
  
  render(<SearchResults products={products} />);
  
  expect(screen.getByText('Product 1')).toBeInTheDocument();
});

test('should handle product click', () => {
  const onProductClick = jest.fn();
  
  render(
    <SearchResults 
      products={products}
      onProductClick={onProductClick}
    />
  );
  
  fireEvent.click(screen.getByText('Product 1'));
  expect(onProductClick).toHaveBeenCalledWith(products[0]);
});
```