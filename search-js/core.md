# Core Module

The core module provides the foundational functionality for interacting with Nosto Search. It includes utilities for managing search queries, applying decorators, handling search results, and implementing advanced features like caching and retry logic.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```typescript
import { search } from '@nosto/search-js/core';

const results = await search({
  query: 'running shoes',
  size: 24,
  from: 0
});
```

## Key Features

### 🔍 Search Functionality

The core search function provides a flexible interface for querying Nosto's search API with comprehensive configuration options.

```typescript
import { search, SearchConfig } from '@nosto/search-js/core';

const config: SearchConfig = {
  query: 'laptops',
  size: 20,
  from: 0,
  facets: ['brand', 'price'],
  sort: [{ field: 'price', order: 'asc' }],
  filters: {
    brand: ['Apple', 'Dell'],
    'price.min': 500
  }
};

const searchResults = await search(config);
```

### 🔄 Retry Logic

Built-in retry mechanism for handling network failures and temporary service issues.

```typescript
import { withRetries } from '@nosto/search-js/core';

const searchWithRetries = withRetries(search, {
  maxRetries: 3,
  retryDelay: 1000,
  backoffFactor: 2
});

const results = await searchWithRetries(config);
```

**Retry Configuration:**
- `maxRetries`: Maximum number of retry attempts (default: 3)
- `retryDelay`: Initial delay between retries in milliseconds (default: 1000)
- `backoffFactor`: Exponential backoff multiplier (default: 2)

### 💾 Caching System

Advanced caching mechanisms to improve performance and reduce API calls.

#### Memory Cache

```typescript
import { withMemoryCache } from '@nosto/search-js/core';

const cachedSearch = withMemoryCache(search, {
  maxSize: 100,
  ttl: 300000 // 5 minutes in milliseconds
});

const results = await cachedSearch(config);
```

#### Custom Cache Implementation

```typescript
import { withCache, CacheInterface } from '@nosto/search-js/core';

class CustomCache implements CacheInterface {
  private storage = new Map();
  
  async get(key: string) {
    return this.storage.get(key);
  }
  
  async set(key: string, value: any, ttl?: number) {
    this.storage.set(key, value);
    if (ttl) {
      setTimeout(() => this.storage.delete(key), ttl);
    }
  }
  
  async delete(key: string) {
    this.storage.delete(key);
  }
  
  async clear() {
    this.storage.clear();
  }
}

const searchWithCustomCache = withCache(search, new CustomCache());
```

### 🎨 Decorators System

Decorators allow you to enhance search results with additional data processing.

```typescript
import { applyDecorators, Decorator } from '@nosto/search-js/core';

// Custom decorator example
const upperCaseDecorator: Decorator = (results) => {
  return results.map(product => ({
    ...product,
    name: product.name.toUpperCase()
  }));
};

const decoratedResults = applyDecorators(searchResults, [
  upperCaseDecorator,
  // Add more decorators as needed
]);
```

### 🛒 Add to Cart Integration

Built-in support for add-to-cart tracking and analytics.

```typescript
import { addToCart } from '@nosto/search-js/core';

const handleAddToCart = async (productId: string, variantId?: string) => {
  await addToCart({
    productId,
    variantId,
    quantity: 1,
    // Additional tracking data
    source: 'search_results'
  });
};
```

## API Reference

### search(config: SearchConfig): Promise<SearchResponse>

Performs a search query against the Nosto Search API.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `config` | `SearchConfig` | Yes | Search configuration object |

**SearchConfig Properties:**

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `query` | `string` | Yes | Search query string |
| `size` | `number` | No | Number of results to return (default: 20) |
| `from` | `number` | No | Starting offset for pagination (default: 0) |
| `facets` | `string[]` | No | Facets to include in response |
| `sort` | `SortOption[]` | No | Sorting configuration |
| `filters` | `Record<string, any>` | No | Filter criteria |
| `decorators` | `Decorator[]` | No | Result decorators to apply |

**Returns:** `Promise<SearchResponse>`

### withRetries(searchFn, options): SearchFunction

Wraps a search function with retry logic.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `searchFn` | `SearchFunction` | Yes | Search function to wrap |
| `options` | `RetryOptions` | No | Retry configuration |

### withMemoryCache(searchFn, options): SearchFunction

Wraps a search function with memory caching.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `searchFn` | `SearchFunction` | Yes | Search function to wrap |
| `options` | `CacheOptions` | No | Cache configuration |

### applyDecorators(results, decorators): SearchResult[]

Applies decorators to search results.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `results` | `SearchResult[]` | Yes | Search results to decorate |
| `decorators` | `Decorator[]` | Yes | Array of decorators to apply |

## Types

### SearchConfig

```typescript
interface SearchConfig {
  query: string;
  size?: number;
  from?: number;
  facets?: string[];
  sort?: SortOption[];
  filters?: Record<string, any>;
  decorators?: Decorator[];
}
```

### SearchResponse

```typescript
interface SearchResponse {
  results: SearchResult[];
  total: number;
  facets: Record<string, FacetResult>;
  suggestions?: string[];
  redirect?: string;
}
```

### SearchResult

```typescript
interface SearchResult {
  id: string;
  name: string;
  description?: string;
  price?: number;
  currency?: string;
  imageUrl?: string;
  url?: string;
  brand?: string;
  categories?: string[];
  [key: string]: any;
}
```

## Error Handling

The core module provides comprehensive error handling with specific error types:

```typescript
import { search, SearchError, NetworkError } from '@nosto/search-js/core';

try {
  const results = await search(config);
} catch (error) {
  if (error instanceof NetworkError) {
    console.error('Network error:', error.message);
  } else if (error instanceof SearchError) {
    console.error('Search error:', error.message);
  } else {
    console.error('Unknown error:', error);
  }
}
```

## Best Practices

1. **Use Caching**: Implement caching for better performance and reduced API calls
2. **Handle Errors**: Always implement proper error handling for network issues
3. **Optimize Queries**: Use appropriate `size` and `from` parameters for pagination
4. **Leverage Decorators**: Use decorators to process results consistently
5. **Configure Retries**: Set up retry logic for production environments

## Examples

### Basic Search with Pagination

```typescript
import { search } from '@nosto/search-js/core';

const performSearch = async (query: string, page: number = 1) => {
  const pageSize = 20;
  const from = (page - 1) * pageSize;
  
  return await search({
    query,
    size: pageSize,
    from,
    facets: ['brand', 'category', 'price']
  });
};
```

### Search with Advanced Filtering

```typescript
const advancedSearch = async () => {
  return await search({
    query: 'smartphone',
    size: 24,
    filters: {
      brand: ['Apple', 'Samsung'],
      'price.min': 200,
      'price.max': 1000,
      category: 'Electronics'
    },
    sort: [
      { field: 'popularity', order: 'desc' },
      { field: 'price', order: 'asc' }
    ]
  });
};
```

### Production-Ready Search Setup

```typescript
import { 
  search, 
  withRetries, 
  withMemoryCache 
} from '@nosto/search-js/core';

// Create a production-ready search function
const productionSearch = withRetries(
  withMemoryCache(search, {
    maxSize: 200,
    ttl: 600000 // 10 minutes
  }),
  {
    maxRetries: 3,
    retryDelay: 1000,
    backoffFactor: 2
  }
);

export { productionSearch as search };
```