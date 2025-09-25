# Preact Legacy Components

The legacy module provides backward compatibility components for existing Search Template implementations and older integration patterns. These components are designed to ease migration from previous versions while maintaining existing functionality.

> ⚠️ **Note**: The components included in this package are intended solely for compatibility purposes. They are not recommended for use in new development or as active components in modern applications. For new projects, please use the modern components from other modules.

## Installation

```bash
npm install @nosto/search-js
```

## Migration Support

### Search Template Migration

Components for migrating from Nosto Search Templates to modern React/Preact components.

```tsx
import { 
  LegacySearchTemplate,
  TemplateCompatibilityLayer,
  migrateSearchTemplate 
} from '@nosto/search-js/preact/legacy';

// Legacy template compatibility
<LegacySearchTemplate
  templateId="legacy-search-template"
  apiVersion="v1"
  fallbackComponent={ModernSearchComponent}
  migrationMode="gradual" // "immediate", "gradual", "fallback-only"
  onMigrationComplete={() => console.log('Migration completed')}
>
  <div data-nosto-template="search-results">
    {/* Legacy template content */}
  </div>
</LegacySearchTemplate>

// Compatibility layer for existing integrations
<TemplateCompatibilityLayer
  legacySelectors={{
    searchBox: '.nosto-search-box',
    results: '.nosto-search-results',
    filters: '.nosto-search-filters'
  }}
  modernComponents={{
    searchBox: 'SearchInput',
    results: 'SearchResults',
    filters: 'FilterSidebar'
  }}
  enableProgressiveMigration={true}
/>
```

### Legacy API Support

Support for older API versions and data formats.

```tsx
import { 
  LegacyApiAdapter,
  convertLegacyResponse,
  useLegacyApiCompatibility 
} from '@nosto/search-js/preact/legacy';

const LegacySearchComponent = () => {
  const {
    search,
    results,
    loading,
    error
  } = useLegacyApiCompatibility({
    apiVersion: 'v1',
    endpoint: '/legacy-api/search',
    transformer: convertLegacyResponse
  });

  return (
    <LegacyApiAdapter
      apiVersion="v1"
      responseTransformer={(response) => ({
        products: response.items || [],
        total: response.count || 0,
        facets: response.filters || {}
      })}
    >
      <SearchResults results={results} />
    </LegacyApiAdapter>
  );
};
```

## Deprecated Components

### LegacyAutocomplete

Backward compatible autocomplete component.

```tsx
import { LegacyAutocomplete } from '@nosto/search-js/preact/legacy';

<LegacyAutocomplete
  // Legacy props
  searchUrl="/search"
  suggestionsUrl="/suggestions"
  minChars={3}
  delay={500}
  
  // Compatibility settings
  useJsonp={true}
  crossOrigin={true}
  legacyEventNames={true}
  
  // Migration helpers
  modernReplacement="AutocompleteInput"
  showMigrationWarning={process.env.NODE_ENV === 'development'}
  
  // Legacy event handlers
  onBeforeSearch={(query) => console.log('Legacy: Before search', query)}
  onSearchComplete={(results) => console.log('Legacy: Search complete', results)}
  
  // Legacy template system
  templates={{
    suggestion: '<div class="suggestion">{{name}}</div>',
    empty: '<div class="empty">No suggestions found</div>'
  }}
/>
```

### LegacySearchResults

Compatible search results component for older implementations.

```tsx
import { LegacySearchResults } from '@nosto/search-js/preact/legacy';

<LegacySearchResults
  // Legacy configuration
  resultsPerPage={20}
  infiniteScroll={false}
  ajaxContainer=".search-results"
  
  // Legacy styling
  cssClasses={{
    container: 'nosto-search-results',
    item: 'search-result-item',
    pagination: 'search-pagination'
  }}
  
  // Legacy templating
  itemTemplate={`
    <div class="product-item">
      <img src="{{image_url}}" alt="{{name}}">
      <h3>{{name}}</h3>
      <p class="price">{{price}}</p>
    </div>
  `}
  
  // Migration settings
  enableModernFeatures={false}
  maintainLegacyStructure={true}
  modernReplacement="SearchResults"
/>
```

### LegacyFilters

Backward compatible filter component.

```tsx
import { LegacyFilters } from '@nosto/search-js/preact/legacy';

<LegacyFilters
  // Legacy filter configuration
  filterTypes={['brand', 'category', 'price']}
  multiSelect={true}
  urlParams={true}
  
  // Legacy styling
  listStyle="checkbox" // "radio", "dropdown", "slider"
  collapsible={true}
  expandedByDefault={false}
  
  // Legacy event system
  onFilterChange={(filters) => {
    // Legacy callback format
    window.nostoLegacyFilters = filters;
    if (window.legacySearchCallback) {
      window.legacySearchCallback(filters);
    }
  }}
  
  // Legacy template
  filterTemplate={`
    <div class="filter-group">
      <h4>{{title}}</h4>
      <ul class="filter-options">
        {{#options}}
        <li>
          <input type="checkbox" id="{{id}}" value="{{value}}">
          <label for="{{id}}">{{name}} ({{count}})</label>
        </li>
        {{/options}}
      </ul>
    </div>
  `}
/>
```

## Migration Utilities

### Gradual Migration Helper

Tool for gradual migration from legacy to modern components.

```tsx
import { GradualMigrationProvider } from '@nosto/search-js/preact/legacy';

<GradualMigrationProvider
  migrationPlan={{
    phase1: {
      components: ['SearchInput'],
      timeline: '2024-Q1',
      features: ['autocomplete', 'voice-search']
    },
    phase2: {
      components: ['SearchResults'],
      timeline: '2024-Q2',
      features: ['infinite-scroll', 'virtual-scrolling']
    },
    phase3: {
      components: ['FilterSidebar'],
      timeline: '2024-Q3',
      features: ['advanced-filters', 'faceted-search']
    }
  }}
  currentPhase="phase1"
  enableProgressiveEnhancement={true}
  fallbackToLegacy={true}
>
  <SearchApplication />
</GradualMigrationProvider>
```

### Configuration Migration

Migrate legacy configuration to modern format.

```tsx
import { 
  migrateLegacyConfig,
  validateLegacyConfig,
  convertConfigFormat 
} from '@nosto/search-js/preact/legacy';

// Legacy configuration
const legacyConfig = {
  account_id: 'legacy-account',
  search_url: '/legacy/search',
  suggestions_enabled: true,
  filters_enabled: true,
  results_per_page: 20,
  infinite_scroll: false
};

// Migrate to modern format
const modernConfig = migrateLegacyConfig(legacyConfig, {
  mapAccountId: (id) => id.replace('legacy-', ''),
  convertUrls: true,
  enableModernFeatures: true,
  preserveCustomSettings: true
});

// Validation
const validationResult = validateLegacyConfig(legacyConfig);
if (!validationResult.valid) {
  console.warn('Legacy config issues:', validationResult.issues);
}

// Format conversion
const convertedConfig = convertConfigFormat(legacyConfig, 'v1', 'v2');
```

## Event System Migration

### Legacy Event Compatibility

Bridge between legacy and modern event systems.

```tsx
import { 
  LegacyEventBridge,
  convertLegacyEvents,
  useLegacyEventCompatibility 
} from '@nosto/search-js/preact/legacy';

const LegacyCompatibleComponent = () => {
  const {
    emitLegacyEvent,
    subscribeLegacyEvent,
    convertToModernEvent
  } = useLegacyEventCompatibility();

  // Listen for legacy events
  useEffect(() => {
    const unsubscribe = subscribeLegacyEvent('nosto:search:complete', (data) => {
      const modernEvent = convertToModernEvent(data);
      handleModernSearch(modernEvent);
    });

    return unsubscribe;
  }, []);

  // Emit legacy events for backward compatibility
  const handleSearch = (query) => {
    performModernSearch(query);
    
    // Emit legacy event for existing integrations
    emitLegacyEvent('nosto:search:started', {
      query,
      timestamp: Date.now()
    });
  };

  return (
    <LegacyEventBridge
      legacyEventPrefix="nosto:"
      modernEventPrefix="nosto-modern:"
      bidirectional={true}
    >
      <SearchInput onSearch={handleSearch} />
    </LegacyEventBridge>
  );
};
```

### Global Event Compatibility

Support for global event systems used in legacy implementations.

```tsx
import { GlobalEventCompatibility } from '@nosto/search-js/preact/legacy';

<GlobalEventCompatibility
  globalObject="window.NostoSearch"
  legacyMethods={{
    search: (query) => modernSearchInstance.search(query),
    getResults: () => modernSearchInstance.getResults(),
    clearSearch: () => modernSearchInstance.clear()
  }}
  
  // jQuery compatibility
  jQueryPlugin={{
    name: 'nostoSearch',
    methods: ['search', 'filter', 'clear'],
    chainable: true
  }}
  
  // Custom global compatibility
  customGlobals={{
    'NOSTO_SEARCH_API': {
      version: '2.0.0',
      search: (query) => modernSearch(query),
      init: (config) => initializeModernSearch(config)
    }
  }}
/>
```

## Data Format Migration

### Legacy Response Transformation

Transform legacy API responses to modern format.

```tsx
import { 
  transformLegacyResponse,
  createResponseTransformer,
  validateResponseFormat 
} from '@nosto/search-js/preact/legacy';

// Legacy response format
const legacyResponse = {
  items: [
    {
      product_id: '123',
      product_name: 'Product Name',
      product_price: '99.99',
      product_image: 'image.jpg'
    }
  ],
  total_count: 1,
  filters: {
    brand: [
      { name: 'Brand A', count: 5 },
      { name: 'Brand B', count: 3 }
    ]
  }
};

// Transform to modern format
const modernResponse = transformLegacyResponse(legacyResponse, {
  productMapping: {
    id: 'product_id',
    name: 'product_name',
    price: 'product_price',
    imageUrl: 'product_image'
  },
  
  facetMapping: {
    brand: {
      values: (items) => items.map(item => ({
        value: item.name.toLowerCase(),
        label: item.name,
        count: item.count
      }))
    }
  }
});

// Custom transformer
const customTransformer = createResponseTransformer({
  version: 'v1',
  transformations: {
    products: (items) => items.map(transformProduct),
    facets: (filters) => transformFilters(filters),
    pagination: (response) => ({
      total: response.total_count,
      page: response.current_page || 1,
      pageSize: response.page_size || 20
    })
  }
});
```

### URL Parameter Migration

Handle legacy URL parameter formats.

```tsx
import { 
  migrateLegacyUrlParams,
  createUrlParamAdapter,
  synchronizeLegacyParams 
} from '@nosto/search-js/preact/legacy';

const LegacyUrlCompatibility = () => {
  // Migrate legacy URL parameters
  useEffect(() => {
    const modernParams = migrateLegacyUrlParams({
      legacyParams: {
        q: 'search query',
        f_brand: 'nike,adidas',
        f_price: '50-100',
        page: '2'
      },
      paramMapping: {
        q: 'query',
        'f_brand': 'filters.brand',
        'f_price': 'filters.price',
        page: 'pagination.page'
      }
    });

    // Apply modern parameters
    applySearchParams(modernParams);
  }, []);

  return <SearchInterface />;
};

// URL adapter
const urlAdapter = createUrlParamAdapter({
  legacyFormat: 'v1',
  modernFormat: 'v2',
  bidirectional: true,
  preserveLegacyParams: true
});
```

## Styling Migration

### CSS Class Migration

Handle legacy CSS class structures.

```tsx
import { 
  LegacyCssCompatibility,
  migrateCssClasses,
  createStyleAdapter 
} from '@nosto/search-js/preact/legacy';

<LegacyCssCompatibility
  legacyClasses={{
    // Legacy class mappings
    'nosto-search-box': 'modern-search-input',
    'nosto-search-results': 'modern-search-results',
    'nosto-product-item': 'modern-product-card'
  }}
  
  // Style preservation
  preserveLegacyStyles={true}
  addCompatibilityClasses={true}
  
  // Migration warnings
  warnOnLegacyClasses={process.env.NODE_ENV === 'development'}
>
  <SearchComponents />
</LegacyCssCompatibility>

// Style adapter
const styleAdapter = createStyleAdapter({
  legacyTheme: 'v1',
  modernTheme: 'v2',
  cssVariableMigration: {
    '--nosto-primary-color': '--search-primary',
    '--nosto-background': '--search-background'
  }
});
```

## Testing Legacy Components

### Compatibility Testing

```tsx
import { 
  testLegacyCompatibility,
  mockLegacyEnvironment,
  validateMigration 
} from '@nosto/search-js/preact/legacy/testing';

describe('Legacy Compatibility', () => {
  test('should maintain legacy API compatibility', async () => {
    const legacyEnv = mockLegacyEnvironment({
      version: 'v1',
      globalObject: 'window.NostoSearch',
      legacyEvents: true
    });

    const compatibilityResult = await testLegacyCompatibility({
      component: LegacySearchComponent,
      environment: legacyEnv,
      expectedBehavior: 'v1'
    });

    expect(compatibilityResult.compatible).toBe(true);
    expect(compatibilityResult.issues).toHaveLength(0);
  });

  test('should validate migration completeness', () => {
    const migrationResult = validateMigration({
      from: 'legacy-template',
      to: 'modern-component',
      features: ['search', 'filters', 'pagination']
    });

    expect(migrationResult.complete).toBe(true);
    expect(migrationResult.missingFeatures).toHaveLength(0);
  });
});
```

## Migration Documentation

### Migration Guide Generator

```tsx
import { generateMigrationGuide } from '@nosto/search-js/preact/legacy';

const migrationGuide = generateMigrationGuide({
  currentImplementation: 'search-template-v1',
  targetImplementation: 'modern-components-v2',
  features: ['search', 'autocomplete', 'filters', 'results'],
  customizations: ['styling', 'events', 'configuration'],
  timeline: '6-months',
  
  generateDocumentation: true,
  includeCodeExamples: true,
  addMigrationChecklist: true
});

console.log(migrationGuide.steps);
console.log(migrationGuide.codeExamples);
console.log(migrationGuide.checklist);
```

## Deprecation Warnings

### Development Warnings

```tsx
import { DeprecationWarnings } from '@nosto/search-js/preact/legacy';

<DeprecationWarnings
  showInDevelopment={true}
  showInProduction={false}
  logToConsole={true}
  
  // Custom warning messages
  messages={{
    'LegacyAutocomplete': 'LegacyAutocomplete is deprecated. Use AutocompleteInput instead.',
    'LegacySearchResults': 'LegacySearchResults will be removed in v3.0. Migrate to SearchResults.',
    'LegacyFilters': 'LegacyFilters is no longer maintained. Use FilterSidebar.'
  }}
  
  // Migration assistance
  showMigrationLinks={true}
  migrationDocs="https://docs.nosto.com/migration-guide"
  
  // Tracking
  trackDeprecationUsage={true}
  onDeprecationUsed={(component, usage) => {
    analytics.track('deprecated_component_used', {
      component,
      usage,
      timestamp: Date.now()
    });
  }}
/>
```

## Best Practices for Legacy Support

1. **Plan Migration**: Create a clear migration plan with timelines
2. **Gradual Transition**: Migrate components gradually, not all at once
3. **Test Thoroughly**: Test legacy compatibility in various environments
4. **Document Changes**: Keep detailed documentation of changes and migrations
5. **Monitor Usage**: Track usage of legacy components to plan deprecation
6. **Provide Support**: Offer migration assistance and documentation
7. **Set Deadlines**: Establish clear end-of-life dates for legacy components

## End-of-Life Timeline

| Component | Deprecated Since | End of Support | Removal Date |
|-----------|------------------|----------------|--------------|
| LegacyAutocomplete | v2.0.0 | v2.5.0 | v3.0.0 |
| LegacySearchResults | v2.0.0 | v2.5.0 | v3.0.0 |
| LegacyFilters | v2.0.0 | v2.5.0 | v3.0.0 |
| TemplateCompatibilityLayer | v2.1.0 | v3.0.0 | v4.0.0 |