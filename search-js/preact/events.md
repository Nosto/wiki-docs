# Preact Events

The events module provides comprehensive event handling, analytics tracking, and user interaction management for search interfaces. It enables tracking of user behavior, search analytics, and integration with various analytics platforms.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { EventProvider, useEventTracking } from '@nosto/search-js/preact/events';
import { SearchProvider } from '@nosto/search-js/preact/common';

const App = () => (
  <SearchProvider config={{ accountId: 'your-account-id' }}>
    <EventProvider 
      config={{
        providers: ['nosto', 'google-analytics', 'custom'],
        enableAutoTracking: true
      }}
    >
      <SearchApp />
    </EventProvider>
  </SearchProvider>
);

const SearchComponent = () => {
  const { trackEvent, trackSearch, trackClick } = useEventTracking();
  
  const handleSearch = (query) => {
    trackSearch(query);
    performSearch(query);
  };
  
  return (
    <SearchInput onSearch={handleSearch} />
  );
};
```

## Core Features

### Event Tracking

Comprehensive tracking of search-related user interactions.

```tsx
import { useEventTracking } from '@nosto/search-js/preact/events';

const ProductCard = ({ product }) => {
  const { 
    trackView,
    trackClick, 
    trackAddToCart,
    trackPurchase,
    trackCustomEvent 
  } = useEventTracking();

  const handleView = useCallback(() => {
    trackView({
      type: 'product_view',
      productId: product.id,
      categoryId: product.categoryId,
      position: product.position,
      query: currentQuery,
      timestamp: Date.now()
    });
  }, [product, currentQuery]);

  const handleClick = () => {
    trackClick({
      type: 'product_click',
      productId: product.id,
      url: product.url,
      position: product.position,
      source: 'search_results'
    });
    
    window.location.href = product.url;
  };

  const handleAddToCart = () => {
    trackAddToCart({
      type: 'add_to_cart',
      productId: product.id,
      variantId: product.selectedVariant?.id,
      quantity: 1,
      price: product.price,
      currency: product.currency
    });
    
    addToCart(product);
  };

  return (
    <div 
      className="product-card"
      onMouseEnter={handleView}
      onClick={handleClick}
    >
      <img src={product.imageUrl} alt={product.name} />
      <h3>{product.name}</h3>
      <p>{product.formattedPrice}</p>
      <button onClick={handleAddToCart}>
        Add to Cart
      </button>
    </div>
  );
};
```

### Search Analytics

Track search-specific metrics and user behavior.

```tsx
import { useSearchAnalytics } from '@nosto/search-js/preact/events';

const SearchInterface = () => {
  const {
    trackSearchQuery,
    trackSearchResults,
    trackNoResults,
    trackFilterUsage,
    trackSortUsage,
    trackPagination,
    getSearchMetrics
  } = useSearchAnalytics();

  const handleSearch = async (query) => {
    const startTime = performance.now();
    
    trackSearchQuery({
      query,
      timestamp: Date.now(),
      sessionId: getSessionId()
    });

    try {
      const results = await performSearch(query);
      const endTime = performance.now();
      
      if (results.length === 0) {
        trackNoResults({
          query,
          filters: activeFilters,
          suggestionShown: showSuggestion
        });
      } else {
        trackSearchResults({
          query,
          resultCount: results.length,
          responseTime: endTime - startTime,
          facets: results.facets,
          suggestions: results.suggestions
        });
      }
      
      setResults(results);
    } catch (error) {
      trackCustomEvent('search_error', {
        query,
        error: error.message,
        timestamp: Date.now()
      });
    }
  };

  const handleFilterChange = (filterType, filterValue) => {
    trackFilterUsage({
      type: filterType,
      value: filterValue,
      action: 'add',
      query: currentQuery
    });
  };

  return (
    <div>
      <SearchInput onSearch={handleSearch} />
      <FilterSidebar onFilterChange={handleFilterChange} />
      <SearchResults results={results} />
    </div>
  );
};
```

### User Journey Tracking

Track complete user journeys through the search interface.

```tsx
import { useJourneyTracking } from '@nosto/search-js/preact/events';

const SearchJourney = () => {
  const {
    startJourney,
    trackStep,
    endJourney,
    getCurrentJourney,
    getJourneyMetrics
  } = useJourneyTracking();

  useEffect(() => {
    startJourney({
      type: 'search_session',
      entryPoint: 'search_page',
      userId: getUserId(),
      sessionId: getSessionId()
    });

    return () => {
      endJourney({
        exitPoint: 'search_results',
        duration: Date.now() - journey.startTime
      });
    };
  }, []);

  const trackSearchStep = (stepType, data) => {
    trackStep({
      type: stepType,
      timestamp: Date.now(),
      data,
      sequence: journey.steps.length + 1
    });
  };

  return (
    <SearchInterface onStep={trackSearchStep} />
  );
};
```

## Analytics Providers

### Nosto Analytics

Built-in integration with Nosto's analytics platform.

```tsx
import { NostoAnalyticsProvider } from '@nosto/search-js/preact/events';

<NostoAnalyticsProvider
  config={{
    accountId: 'your-nosto-account',
    apiKey: 'your-api-key',
    enableAutoTracking: true,
    trackingMode: 'client', // 'server', 'hybrid'
    batchSize: 10,
    flushInterval: 5000
  }}
>
  <SearchApp />
</NostoAnalyticsProvider>
```

### Google Analytics

Integration with Google Analytics 4.

```tsx
import { GoogleAnalyticsProvider } from '@nosto/search-js/preact/events';

<GoogleAnalyticsProvider
  config={{
    measurementId: 'G-XXXXXXXXXX',
    enableEnhancedEcommerce: true,
    customDimensions: {
      searchQuery: 'custom_dimension_1',
      categoryId: 'custom_dimension_2'
    },
    eventMapping: {
      search: 'search',
      view_item: 'view_item',
      add_to_cart: 'add_to_cart',
      purchase: 'purchase'
    }
  }}
>
  <SearchApp />
</GoogleAnalyticsProvider>
```

### Custom Analytics

Create custom analytics providers.

```tsx
import { createAnalyticsProvider } from '@nosto/search-js/preact/events';

const CustomAnalyticsProvider = createAnalyticsProvider({
  name: 'custom',
  
  initialize: (config) => {
    // Initialize your analytics service
    window.customAnalytics.init(config.apiKey);
  },
  
  track: (eventType, eventData) => {
    // Send event to your analytics service
    window.customAnalytics.track(eventType, {
      ...eventData,
      timestamp: Date.now(),
      source: 'nosto-search-js'
    });
  },
  
  identify: (userId, traits) => {
    // Identify user
    window.customAnalytics.identify(userId, traits);
  },
  
  page: (name, properties) => {
    // Track page view
    window.customAnalytics.page(name, properties);
  }
});

<CustomAnalyticsProvider config={{ apiKey: 'your-key' }}>
  <SearchApp />
</CustomAnalyticsProvider>
```

## Event Types

### Search Events

```tsx
// Search query event
trackEvent('search_query', {
  query: 'running shoes',
  resultCount: 1247,
  responseTime: 156,
  filters: { brand: ['nike'], price: '50-150' },
  suggestions: ['running sneakers', 'athletic shoes']
});

// No results event
trackEvent('search_no_results', {
  query: 'rare vintage item',
  filters: { category: 'vintage' },
  suggestionsShown: true,
  alternativeQueries: ['vintage items', 'rare collectibles']
});

// Search refinement
trackEvent('search_refinement', {
  originalQuery: 'shoes',
  refinedQuery: 'running shoes',
  refinementType: 'autocomplete'
});
```

### Product Interaction Events

```tsx
// Product view
trackEvent('product_view', {
  productId: 'prod_123',
  productName: 'Running Shoes',
  categoryId: 'shoes',
  price: 99.99,
  currency: 'USD',
  position: 3,
  listName: 'Search Results'
});

// Product click
trackEvent('product_click', {
  productId: 'prod_123',
  url: '/products/running-shoes',
  position: 3,
  source: 'search_results',
  query: 'running shoes'
});

// Add to cart
trackEvent('add_to_cart', {
  productId: 'prod_123',
  variantId: 'var_456',
  quantity: 1,
  price: 99.99,
  currency: 'USD',
  source: 'search_results'
});
```

### Filter and Sort Events

```tsx
// Filter usage
trackEvent('filter_applied', {
  filterType: 'brand',
  filterValue: 'nike',
  query: 'shoes',
  resultCountBefore: 1000,
  resultCountAfter: 245
});

// Sort usage
trackEvent('sort_applied', {
  sortBy: 'price',
  sortOrder: 'asc',
  query: 'shoes',
  previousSort: 'relevance'
});

// Pagination
trackEvent('pagination', {
  page: 2,
  pageSize: 24,
  totalResults: 1247,
  query: 'shoes'
});
```

## Performance Tracking

### Search Performance Metrics

```tsx
import { usePerformanceTracking } from '@nosto/search-js/preact/events';

const SearchWithPerformance = () => {
  const {
    trackSearchPerformance,
    trackRenderPerformance,
    trackUserInteraction
  } = usePerformanceTracking();

  const handleSearch = async (query) => {
    const performanceMarker = performance.mark('search-start');
    
    try {
      const results = await performSearch(query);
      
      performance.mark('search-end');
      const searchTime = performance.measure('search-duration', 'search-start', 'search-end');
      
      trackSearchPerformance({
        query,
        duration: searchTime.duration,
        resultCount: results.length,
        cacheHit: results.fromCache
      });
      
    } catch (error) {
      trackSearchPerformance({
        query,
        error: error.message,
        duration: performance.now() - performanceMarker.startTime
      });
    }
  };

  return <SearchInterface onSearch={handleSearch} />;
};
```

### Component Performance

```tsx
import { withPerformanceTracking } from '@nosto/search-js/preact/events';

const PerformanceTrackedComponent = withPerformanceTracking(
  ProductCard,
  {
    trackRender: true,
    trackInteractions: true,
    componentName: 'ProductCard'
  }
);

// Usage
<PerformanceTrackedComponent 
  product={product}
  onRenderTime={(duration) => console.log(`Render time: ${duration}ms`)}
/>
```

## A/B Testing Integration

### Experiment Tracking

```tsx
import { useExperimentTracking } from '@nosto/search-js/preact/events';

const ExperimentalSearchResults = () => {
  const {
    getExperiment,
    trackExperimentView,
    trackExperimentConversion
  } = useExperimentTracking();

  const experiment = getExperiment('search-layout-test');
  
  useEffect(() => {
    trackExperimentView(experiment);
  }, [experiment]);

  const handleProductClick = (product) => {
    trackExperimentConversion(experiment, {
      type: 'product_click',
      productId: product.id,
      value: product.price
    });
  };

  return (
    <div className={`layout-${experiment.variant}`}>
      <SearchResults onProductClick={handleProductClick} />
    </div>
  );
};
```

## Real-time Analytics

### Live Metrics Dashboard

```tsx
import { useRealTimeAnalytics } from '@nosto/search-js/preact/events';

const AnalyticsDashboard = () => {
  const {
    liveMetrics,
    subscribe,
    unsubscribe
  } = useRealTimeAnalytics();

  useEffect(() => {
    const unsubscribeSearches = subscribe('search_queries', (data) => {
      updateSearchMetrics(data);
    });

    const unsubscribeClicks = subscribe('product_clicks', (data) => {
      updateClickMetrics(data);
    });

    return () => {
      unsubscribeSearches();
      unsubscribeClicks();
    };
  }, []);

  return (
    <div className="analytics-dashboard">
      <MetricCard 
        title="Searches per minute"
        value={liveMetrics.searchesPerMinute}
      />
      <MetricCard 
        title="Click-through rate"
        value={`${liveMetrics.clickThroughRate}%`}
      />
      <MetricCard 
        title="Average response time"
        value={`${liveMetrics.avgResponseTime}ms`}
      />
    </div>
  );
};
```

## Data Privacy

### GDPR Compliance

```tsx
import { PrivacyProvider } from '@nosto/search-js/preact/events';

<PrivacyProvider
  config={{
    enableCookieConsent: true,
    anonymizeIPs: true,
    dataRetentionDays: 365,
    consentTypes: ['analytics', 'personalization'],
    onConsentChange: (consent) => {
      updateTrackingSettings(consent);
    }
  }}
>
  <SearchApp />
</PrivacyProvider>
```

### Data Anonymization

```tsx
import { useDataPrivacy } from '@nosto/search-js/preact/events';

const PrivacyAwareTracking = () => {
  const { 
    hasConsent, 
    anonymizeData, 
    maskSensitiveData 
  } = useDataPrivacy();

  const trackSearchEvent = (eventData) => {
    if (!hasConsent('analytics')) {
      return; // Don't track without consent
    }

    const sanitizedData = {
      ...anonymizeData(eventData, ['userId', 'email']),
      query: maskSensitiveData(eventData.query)
    };

    trackEvent('search', sanitizedData);
  };

  return <SearchInterface onSearch={trackSearchEvent} />;
};
```

## Configuration

### Event Provider Configuration

```tsx
const eventConfig = {
  providers: [
    {
      name: 'nosto',
      config: { accountId: 'your-account' },
      enabled: true
    },
    {
      name: 'google-analytics',
      config: { measurementId: 'G-XXXXXXXXXX' },
      enabled: process.env.NODE_ENV === 'production'
    }
  ],
  
  // Global settings
  enableAutoTracking: true,
  batchEvents: true,
  batchSize: 10,
  flushInterval: 5000,
  retryFailedEvents: true,
  maxRetries: 3,
  
  // Privacy settings
  respectDoNotTrack: true,
  anonymizeIPs: true,
  cookieConsent: true,
  
  // Debug settings
  debugMode: process.env.NODE_ENV === 'development',
  logEvents: true
};

<EventProvider config={eventConfig}>
  <SearchApp />
</EventProvider>
```

## Testing

### Event Testing Utilities

```tsx
import { 
  mockEventProvider,
  expectEventTracked,
  getTrackedEvents 
} from '@nosto/search-js/preact/events/testing';

test('should track search event', async () => {
  const { trackEvent } = mockEventProvider();
  
  render(
    <EventProvider tracker={trackEvent}>
      <SearchComponent />
    </EventProvider>
  );

  fireEvent.input(screen.getByRole('textbox'), { 
    target: { value: 'test query' } 
  });

  await expectEventTracked('search_query', {
    query: 'test query'
  });

  const events = getTrackedEvents();
  expect(events).toHaveLength(1);
});
```

## Best Practices

1. **Batch Events**: Use event batching for better performance
2. **Respect Privacy**: Always check for user consent before tracking
3. **Error Handling**: Implement proper error handling for tracking failures
4. **Performance**: Track performance metrics to optimize user experience
5. **Data Quality**: Validate and sanitize event data before sending
6. **Testing**: Mock event tracking in tests to avoid sending test data
7. **Documentation**: Document custom events and their data structure