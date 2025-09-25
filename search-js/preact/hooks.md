# Preact Hooks

The hooks module provides a comprehensive set of React/Preact hooks for managing search state, handling user interactions, and integrating with Nosto's search functionality. These hooks follow React patterns and provide clean, composable APIs for building search interfaces.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { useSearch, useFacets, usePagination } from '@nosto/search-js/preact/hooks';

const SearchPage = () => {
  const { search, results, loading, error } = useSearch();
  const { facets, toggleFacet, clearFacets } = useFacets();
  const { currentPage, totalPages, goToPage } = usePagination();

  return (
    <div>
      <SearchInput onSearch={search} />
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

## Core Hooks

### useSearch

Main hook for search functionality and state management.

```tsx
import { useSearch } from '@nosto/search-js/preact/hooks';

const SearchComponent = () => {
  const {
    // State
    query,
    results,
    loading,
    error,
    total,
    suggestions,
    
    // Actions
    search,
    clearSearch,
    setQuery,
    
    // Configuration
    config,
    updateConfig
  } = useSearch({
    initialQuery: '',
    autoSearch: true,
    debounce: 300,
    minLength: 2
  });

  return (
    <div>
      <input 
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Search..."
      />
      {loading && <div>Searching...</div>}
      {error && <div>Error: {error.message}</div>}
      <div>{total} results found</div>
      {results.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
};
```

### useFacets

Hook for managing search facets and filters.

```tsx
import { useFacets } from '@nosto/search-js/preact/hooks';

const FacetsComponent = () => {
  const {
    facets,
    selectedFacets,
    toggleFacet,
    selectFacet,
    deselectFacet,
    clearFacets,
    clearFacetGroup,
    isFacetSelected,
    getSelectedCount
  } = useFacets();

  return (
    <div className="facets">
      {Object.entries(facets).map(([groupName, facetGroup]) => (
        <div key={groupName} className="facet-group">
          <h3>
            {groupName}
            <button onClick={() => clearFacetGroup(groupName)}>
              Clear ({getSelectedCount(groupName)})
            </button>
          </h3>
          {facetGroup.values.map(facet => (
            <label key={facet.value}>
              <input
                type="checkbox"
                checked={isFacetSelected(groupName, facet.value)}
                onChange={() => toggleFacet(groupName, facet.value)}
              />
              {facet.name} ({facet.count})
            </label>
          ))}
        </div>
      ))}
    </div>
  );
};
```

### usePagination

Hook for handling pagination logic.

```tsx
import { usePagination } from '@nosto/search-js/preact/hooks';

const PaginationComponent = () => {
  const {
    currentPage,
    totalPages,
    pageSize,
    total,
    hasNextPage,
    hasPreviousPage,
    goToPage,
    nextPage,
    previousPage,
    goToFirstPage,
    goToLastPage,
    setPageSize,
    getPageNumbers
  } = usePagination({
    initialPage: 1,
    initialPageSize: 20,
    maxPageNumbers: 5
  });

  return (
    <div className="pagination">
      <div className="pagination-info">
        Showing {(currentPage - 1) * pageSize + 1} - {Math.min(currentPage * pageSize, total)} of {total} results
      </div>
      
      <div className="pagination-controls">
        <button 
          onClick={goToFirstPage}
          disabled={!hasPreviousPage}
        >
          First
        </button>
        
        <button 
          onClick={previousPage}
          disabled={!hasPreviousPage}
        >
          Previous
        </button>
        
        {getPageNumbers().map(pageNum => (
          <button
            key={pageNum}
            onClick={() => goToPage(pageNum)}
            className={pageNum === currentPage ? 'active' : ''}
          >
            {pageNum}
          </button>
        ))}
        
        <button 
          onClick={nextPage}
          disabled={!hasNextPage}
        >
          Next
        </button>
        
        <button 
          onClick={goToLastPage}
          disabled={!hasNextPage}
        >
          Last
        </button>
      </div>
      
      <select 
        value={pageSize} 
        onChange={(e) => setPageSize(Number(e.target.value))}
      >
        <option value={10}>10 per page</option>
        <option value={20}>20 per page</option>
        <option value={50}>50 per page</option>
      </select>
    </div>
  );
};
```

### useSort

Hook for managing search result sorting.

```tsx
import { useSort } from '@nosto/search-js/preact/hooks';

const SortComponent = () => {
  const {
    sortBy,
    sortOrder,
    availableSorts,
    setSortBy,
    setSortOrder,
    toggleSortOrder,
    clearSort
  } = useSort({
    initialSort: 'relevance',
    initialOrder: 'desc'
  });

  return (
    <div className="sort-controls">
      <select 
        value={sortBy} 
        onChange={(e) => setSortBy(e.target.value)}
      >
        {availableSorts.map(sort => (
          <option key={sort.field} value={sort.field}>
            {sort.label}
          </option>
        ))}
      </select>
      
      <button onClick={toggleSortOrder}>
        {sortOrder === 'asc' ? '↑' : '↓'}
      </button>
      
      <button onClick={clearSort}>
        Clear Sort
      </button>
    </div>
  );
};
```

## Advanced Hooks

### useLoadMore

Hook for infinite scroll and load more functionality.

```tsx
import { useLoadMore } from '@nosto/search-js/preact/hooks';

const InfiniteScrollResults = () => {
  const {
    results,
    loading,
    hasMore,
    loadMore,
    reset,
    isLoadingMore
  } = useLoadMore({
    pageSize: 20,
    threshold: 200 // Load more when 200px from bottom
  });

  return (
    <div className="infinite-scroll">
      {results.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
      
      {isLoadingMore && <div>Loading more...</div>}
      
      {hasMore && !loading && (
        <button onClick={loadMore}>
          Load More
        </button>
      )}
      
      {!hasMore && <div>No more results</div>}
    </div>
  );
};
```

### usePersonalization

Hook for managing personalized search experiences.

```tsx
import { usePersonalization } from '@nosto/search-js/preact/hooks';

const PersonalizedSearch = () => {
  const {
    preferences,
    recommendations,
    recentlyViewed,
    updatePreferences,
    trackView,
    clearHistory
  } = usePersonalization();

  return (
    <div>
      <div className="recommendations">
        <h3>Recommended for You</h3>
        {recommendations.map(product => (
          <ProductCard 
            key={product.id} 
            product={product}
            onView={() => trackView(product)}
          />
        ))}
      </div>
      
      <div className="recently-viewed">
        <h3>Recently Viewed</h3>
        {recentlyViewed.map(product => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>
    </div>
  );
};
```

### useActions

Hook for handling user actions and analytics.

```tsx
import { useActions } from '@nosto/search-js/preact/hooks';

const ProductCard = ({ product }) => {
  const {
    trackClick,
    trackView,
    trackAddToCart,
    trackPurchase,
    trackCustomEvent
  } = useActions();

  const handleClick = () => {
    trackClick(product);
    window.location.href = product.url;
  };

  const handleAddToCart = () => {
    trackAddToCart(product);
    addToCart(product);
  };

  return (
    <div 
      className="product-card"
      onMouseEnter={() => trackView(product)}
    >
      <img 
        src={product.imageUrl} 
        alt={product.name}
        onClick={handleClick}
      />
      <h3 onClick={handleClick}>{product.name}</h3>
      <p>{product.formattedPrice}</p>
      <button onClick={handleAddToCart}>
        Add to Cart
      </button>
    </div>
  );
};
```

### useRange

Hook for range filters (price, ratings, etc.).

```tsx
import { useRange } from '@nosto/search-js/preact/hooks';

const PriceRangeFilter = () => {
  const {
    min,
    max,
    selectedMin,
    selectedMax,
    setMin,
    setMax,
    setRange,
    clearRange,
    isActive
  } = useRange({
    field: 'price',
    initialMin: 0,
    initialMax: 1000
  });

  return (
    <div className="price-range">
      <h3>Price Range</h3>
      <div className="range-inputs">
        <input
          type="number"
          value={selectedMin}
          onChange={(e) => setMin(Number(e.target.value))}
          min={min}
          max={max}
          placeholder="Min"
        />
        <input
          type="number"
          value={selectedMax}
          onChange={(e) => setMax(Number(e.target.value))}
          min={min}
          max={max}
          placeholder="Max"
        />
      </div>
      
      <input
        type="range"
        min={min}
        max={max}
        value={selectedMin}
        onChange={(e) => setMin(Number(e.target.value))}
      />
      
      <input
        type="range"
        min={min}
        max={max}
        value={selectedMax}
        onChange={(e) => setMax(Number(e.target.value))}
      />
      
      {isActive && (
        <button onClick={clearRange}>
          Clear Price Filter
        </button>
      )}
    </div>
  );
};
```

### useSpeechToText

Hook for voice search functionality.

```tsx
import { useSpeechToText } from '@nosto/search-js/preact/hooks';

const VoiceSearch = () => {
  const {
    transcript,
    listening,
    supported,
    startListening,
    stopListening,
    resetTranscript,
    browserSupportsSpeechRecognition
  } = useSpeechToText({
    language: 'en-US',
    continuous: false,
    interimResults: true
  });

  if (!browserSupportsSpeechRecognition) {
    return <div>Speech recognition not supported</div>;
  }

  return (
    <div className="voice-search">
      <button
        onClick={listening ? stopListening : startListening}
        className={listening ? 'listening' : ''}
      >
        {listening ? '🎤 Listening...' : '🎤 Start Voice Search'}
      </button>
      
      {transcript && (
        <div className="transcript">
          <p>You said: "{transcript}"</p>
          <button onClick={resetTranscript}>Clear</button>
        </div>
      )}
    </div>
  );
};
```

## Hook Combinations

### Complete Search Interface

```tsx
import { 
  useSearch, 
  useFacets, 
  usePagination, 
  useSort,
  useActions 
} from '@nosto/search-js/preact/hooks';

const CompleteSearchInterface = () => {
  const search = useSearch();
  const facets = useFacets();
  const pagination = usePagination();
  const sort = useSort();
  const actions = useActions();

  const handleSearch = (query) => {
    search.search(query);
    actions.trackCustomEvent('search', { query });
  };

  return (
    <div className="search-interface">
      <SearchInput onSearch={handleSearch} />
      
      <div className="search-controls">
        <SortSelector {...sort} />
        <div className="results-count">
          {search.total} results found
        </div>
      </div>
      
      <div className="search-content">
        <aside className="filters">
          <FacetFilters {...facets} />
        </aside>
        
        <main className="results">
          <SearchResults 
            results={search.results}
            loading={search.loading}
            onProductClick={(product) => actions.trackClick(product)}
          />
          <Pagination {...pagination} />
        </main>
      </div>
    </div>
  );
};
```

### Mobile-Optimized Search

```tsx
import { 
  useSearch, 
  useFacets, 
  useLoadMore 
} from '@nosto/search-js/preact/hooks';

const MobileSearch = () => {
  const search = useSearch();
  const facets = useFacets();
  const loadMore = useLoadMore();

  return (
    <div className="mobile-search">
      <div className="search-header">
        <SearchInput onSearch={search.search} />
        <FilterToggle facetsCount={facets.getSelectedCount()} />
      </div>
      
      <InfiniteScrollResults 
        results={loadMore.results}
        onLoadMore={loadMore.loadMore}
        hasMore={loadMore.hasMore}
      />
      
      <FilterModal 
        facets={facets.facets}
        onToggleFacet={facets.toggleFacet}
      />
    </div>
  );
};
```

## Performance Optimization

### Memoization

```tsx
import { useMemo } from 'preact/hooks';
import { useSearch, useFacets } from '@nosto/search-js/preact/hooks';

const OptimizedSearch = () => {
  const search = useSearch();
  const facets = useFacets();

  // Memoize expensive calculations
  const filteredResults = useMemo(() => {
    return search.results.filter(product => 
      product.price >= minPrice && product.price <= maxPrice
    );
  }, [search.results, minPrice, maxPrice]);

  const facetCounts = useMemo(() => {
    return Object.entries(facets.facets).reduce((acc, [key, facet]) => {
      acc[key] = facet.values.reduce((sum, value) => sum + value.count, 0);
      return acc;
    }, {});
  }, [facets.facets]);

  return (
    <div>
      <SearchResults results={filteredResults} />
      <FacetSummary counts={facetCounts} />
    </div>
  );
};
```

### Debounced Updates

```tsx
import { useCallback } from 'preact/hooks';
import { debounce } from '@nosto/search-js/utils';
import { useSearch } from '@nosto/search-js/preact/hooks';

const DebouncedSearch = () => {
  const { search } = useSearch();

  const debouncedSearch = useCallback(
    debounce((query) => search(query), 300),
    [search]
  );

  return (
    <input
      onChange={(e) => debouncedSearch(e.target.value)}
      placeholder="Search..."
    />
  );
};
```

## TypeScript Support

All hooks are fully typed with comprehensive TypeScript definitions:

```tsx
import type {
  UseSearchOptions,
  UseSearchResult,
  UseFacetsResult,
  UsePaginationOptions,
  SearchHookConfig
} from '@nosto/search-js/preact/hooks';

const TypedSearchComponent: React.FC = () => {
  const searchOptions: UseSearchOptions = {
    initialQuery: '',
    autoSearch: true,
    debounce: 300
  };

  const search: UseSearchResult = useSearch(searchOptions);

  return <div>{/* Component JSX */}</div>;
};
```

## Testing Hooks

```tsx
import { renderHook, act } from '@testing-library/preact-hooks';
import { useSearch } from '@nosto/search-js/preact/hooks';

test('should perform search', async () => {
  const { result } = renderHook(() => useSearch());

  act(() => {
    result.current.search('test query');
  });

  expect(result.current.loading).toBe(true);
  expect(result.current.query).toBe('test query');

  await waitFor(() => {
    expect(result.current.loading).toBe(false);
    expect(result.current.results).toHaveLength(10);
  });
});
```

## Best Practices

1. **Use Context Providers**: Wrap your app with SearchProvider for state sharing
2. **Combine Hooks**: Use multiple hooks together for complex functionality
3. **Memoize Expensive Operations**: Use useMemo for heavy calculations
4. **Handle Loading States**: Always show loading indicators during searches
5. **Error Handling**: Implement proper error boundaries and error states
6. **Accessibility**: Ensure hooks support keyboard navigation and screen readers
7. **Performance**: Debounce user inputs and implement pagination for large result sets