# Utils Module

The utils module provides a comprehensive collection of helper functions and utilities for common development tasks in search applications. These utilities cover everything from object manipulation and array operations to performance monitoring and browser detection.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```typescript
import { 
  deepMerge, 
  debounce, 
  isEqual,
  unique, 
  range 
} from '@nosto/search-js/utils';

// Deep merge objects
const merged = deepMerge(obj1, obj2);

// Debounce function calls
const debouncedSearch = debounce(searchFunction, 300);

// Check object equality
const areEqual = isEqual(obj1, obj2);
```

## Key Features

### 🔧 Object Utilities

Powerful utilities for object manipulation and comparison.

```typescript
import { 
  deepMerge, 
  isEqual, 
  pick, 
  isPlainObject,
  deepFreeze 
} from '@nosto/search-js/utils';

// Deep merge multiple objects
const config = deepMerge(
  defaultConfig,
  userConfig,
  { override: true }
);

// Compare objects for deep equality
const isDataChanged = !isEqual(prevData, newData);

// Pick specific properties from object
const subset = pick(user, ['id', 'name', 'email']);

// Check if value is a plain object
if (isPlainObject(data)) {
  // Process object
}

// Prevent object mutation
const immutableData = deepFreeze(data);
```

### 📋 Array Utilities

Efficient array manipulation and processing functions.

```typescript
import { 
  unique, 
  mergeArrays, 
  range 
} from '@nosto/search-js/utils';

// Remove duplicates from array
const uniqueItems = unique(['a', 'b', 'a', 'c']);
// Result: ['a', 'b', 'c']

// Merge arrays with deduplication
const combined = mergeArrays(
  ['apple', 'banana'],
  ['banana', 'cherry'],
  ['apple', 'date']
);
// Result: ['apple', 'banana', 'cherry', 'date']

// Generate number ranges
const pageNumbers = range(1, 10);
// Result: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

const evenNumbers = range(2, 20, 2);
// Result: [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

### ⏱️ Performance Utilities

Tools for performance monitoring and optimization.

```typescript
import { 
  debounce, 
  performance,
  logger 
} from '@nosto/search-js/utils';

// Debounce expensive operations
const debouncedSave = debounce((data) => {
  saveToServer(data);
}, 500);

// Performance monitoring
const timer = performance.start('search-operation');
await performSearch();
const duration = timer.end();
logger.info(`Search completed in ${duration}ms`);

// Advanced debouncing with options
const advancedDebounce = debounce(
  searchFunction,
  300,
  { 
    leading: true,  // Execute immediately on first call
    trailing: true, // Execute after delay
    maxWait: 1000   // Maximum wait time
  }
);
```

### 🌐 Browser Utilities

Browser detection and DOM manipulation helpers.

```typescript
import { 
  isBot, 
  bindInput,
  disableNativeAutocomplete,
  savePageScroll 
} from '@nosto/search-js/utils';

// Detect bot/crawler
if (isBot()) {
  // Handle bot differently
  return serverSideRender();
}

// Bind input element to search
const unbind = bindInput(inputElement, {
  onInput: (value) => performSearch(value),
  debounce: 300,
  minLength: 2
});

// Disable browser autocomplete
disableNativeAutocomplete(inputElement);

// Save/restore scroll position
savePageScroll('search-results');
```

### 💾 Storage Utilities

Local storage management with error handling and expiration.

```typescript
import { storage } from '@nosto/search-js/utils';

// Store data with expiration
storage.set('user-preferences', userData, { 
  ttl: 3600000 // 1 hour in milliseconds 
});

// Retrieve data
const preferences = storage.get('user-preferences');

// Store complex objects
storage.setObject('search-history', searchHistory);
const history = storage.getObject('search-history', []);

// Clear expired items
storage.cleanup();
```

## API Reference

### Object Utilities

#### deepMerge(target: object, ...sources: object[]): object

Deeply merges multiple objects, combining nested properties.

```typescript
const result = deepMerge(
  { a: { x: 1 }, b: 2 },
  { a: { y: 2 }, c: 3 }
);
// Result: { a: { x: 1, y: 2 }, b: 2, c: 3 }
```

#### isEqual(a: any, b: any): boolean

Performs deep equality comparison between two values.

```typescript
const equal = isEqual(
  { a: [1, 2], b: { c: 3 } },
  { a: [1, 2], b: { c: 3 } }
); // true
```

#### pick(object: object, keys: string[]): object

Creates new object with only specified properties.

```typescript
const subset = pick(
  { name: 'John', age: 30, email: 'john@example.com' },
  ['name', 'email']
);
// Result: { name: 'John', email: 'john@example.com' }
```

#### deepFreeze(object: T): T

Recursively freezes an object to prevent mutations.

```typescript
const frozen = deepFreeze({ 
  settings: { theme: 'dark' } 
});
// Object and nested objects are immutable
```

### Array Utilities

#### unique(array: T[]): T[]

Removes duplicate values from an array.

```typescript
const uniqueValues = unique([1, 2, 2, 3, 1]);
// Result: [1, 2, 3]
```

#### mergeArrays(...arrays: T[][]): T[]

Merges multiple arrays with deduplication.

```typescript
const merged = mergeArrays([1, 2], [2, 3], [3, 4]);
// Result: [1, 2, 3, 4]
```

#### range(start: number, end: number, step?: number): number[]

Generates array of numbers in specified range.

```typescript
const numbers = range(1, 5); // [1, 2, 3, 4, 5]
const evens = range(0, 10, 2); // [0, 2, 4, 6, 8, 10]
```

### Performance Utilities

#### debounce(func: Function, delay: number, options?: DebounceOptions): Function

Creates debounced version of function that delays execution.

```typescript
interface DebounceOptions {
  leading?: boolean;   // Execute on leading edge
  trailing?: boolean;  // Execute on trailing edge
  maxWait?: number;   // Maximum wait time
}

const debouncedFn = debounce(expensiveFunction, 300, {
  leading: true,
  maxWait: 1000
});
```

#### performance

Performance monitoring utilities.

```typescript
// Start timer
const timer = performance.start('operation-name');

// End timer and get duration
const duration = timer.end(); // Returns duration in milliseconds

// Mark performance points
performance.mark('search-start');
// ... perform search
performance.mark('search-end');
const searchTime = performance.measure('search-start', 'search-end');
```

### String Utilities

#### simplify(text: string): string

Simplifies text for search comparison by removing accents and normalizing.

```typescript
const simplified = simplify('Café naïve résumé');
// Result: 'cafe naive resume'
```

#### parseNumber(value: string | number): number | null

Safely parses numeric values from strings.

```typescript
const num1 = parseNumber('123.45'); // 123.45
const num2 = parseNumber('$99.99'); // 99.99
const num3 = parseNumber('invalid'); // null
```

### Browser Utilities

#### isBot(): boolean

Detects if the current user agent is a bot or crawler.

```typescript
if (isBot()) {
  // Provide static content for SEO
  return renderStaticContent();
}
```

#### bindInput(element: HTMLInputElement, options: BindInputOptions): () => void

Binds input element with search functionality.

```typescript
interface BindInputOptions {
  onInput?: (value: string) => void;
  onChange?: (value: string) => void;
  debounce?: number;
  minLength?: number;
  maxLength?: number;
}

const unbind = bindInput(searchInput, {
  onInput: (value) => performSearch(value),
  debounce: 300,
  minLength: 2
});

// Cleanup when component unmounts
unbind();
```

#### disableNativeAutocomplete(element: HTMLInputElement): void

Disables browser's native autocomplete on input element.

```typescript
disableNativeAutocomplete(searchInput);
```

### Storage Utilities

#### storage

Wrapper around localStorage with enhanced functionality.

```typescript
interface StorageOptions {
  ttl?: number;        // Time to live in milliseconds
  compress?: boolean;  // Compress stored data
  encrypt?: boolean;   // Encrypt stored data
}

// Basic operations
storage.set('key', 'value');
storage.get('key'); // Returns 'value'
storage.delete('key');
storage.clear();

// With options
storage.set('data', complexObject, { 
  ttl: 3600000,     // 1 hour
  compress: true 
});

// Object operations
storage.setObject('config', { theme: 'dark' });
const config = storage.getObject('config', { theme: 'light' });

// Cleanup expired items
storage.cleanup();
```

## Examples

### Debounced Search Implementation

```typescript
import { debounce, bindInput } from '@nosto/search-js/utils';
import { search } from '@nosto/search-js/core';

const performSearch = debounce(async (query: string) => {
  if (query.length < 2) return;
  
  try {
    const results = await search({ query });
    displayResults(results);
  } catch (error) {
    console.error('Search failed:', error);
  }
}, 300);

// Bind to input element
const searchInput = document.getElementById('search') as HTMLInputElement;
bindInput(searchInput, {
  onInput: performSearch,
  minLength: 2
});
```

### Configuration Management

```typescript
import { deepMerge, storage } from '@nosto/search-js/utils';

class ConfigManager {
  private defaultConfig = {
    search: {
      debounce: 300,
      minLength: 2,
      maxResults: 20
    },
    ui: {
      theme: 'light',
      animations: true
    }
  };

  getConfig() {
    const userConfig = storage.getObject('user-config', {});
    return deepMerge(this.defaultConfig, userConfig);
  }

  updateConfig(updates: any) {
    const currentConfig = this.getConfig();
    const newConfig = deepMerge(currentConfig, updates);
    storage.setObject('user-config', newConfig);
    return newConfig;
  }
}
```

### Array Processing Pipeline

```typescript
import { unique, mergeArrays, range } from '@nosto/search-js/utils';

class SearchHistoryManager {
  private history: string[] = [];

  addSearches(searches: string[]) {
    // Remove duplicates and merge with existing history
    this.history = unique(mergeArrays(this.history, searches));
    
    // Keep only last 50 searches
    if (this.history.length > 50) {
      this.history = this.history.slice(-50);
    }
  }

  getRecentSearches(count: number = 10) {
    return this.history.slice(-count).reverse();
  }

  getPaginatedHistory(page: number, pageSize: number = 10) {
    const start = (page - 1) * pageSize;
    const end = start + pageSize;
    return this.history.slice(start, end);
  }
}
```

### Performance Monitoring

```typescript
import { performance, logger } from '@nosto/search-js/utils';

class SearchPerformanceMonitor {
  async monitoredSearch(query: string) {
    const timer = performance.start(`search-${query}`);
    
    try {
      const results = await search({ query });
      const duration = timer.end();
      
      logger.info(`Search for "${query}" completed in ${duration}ms`);
      
      // Track performance metrics
      this.trackMetrics({
        query,
        duration,
        resultCount: results.length,
        timestamp: Date.now()
      });
      
      return results;
    } catch (error) {
      timer.end();
      logger.error(`Search for "${query}" failed:`, error);
      throw error;
    }
  }

  private trackMetrics(metrics: any) {
    // Send to analytics service
    analytics.track('search_performance', metrics);
  }
}
```

## Best Practices

1. **Use Debouncing**: Always debounce user input for search functionality
2. **Cache Appropriately**: Use storage utilities for user preferences and search history
3. **Monitor Performance**: Track search performance in production
4. **Handle Errors**: Implement proper error handling for all utility functions
5. **Clean Up**: Always clean up event listeners and intervals
6. **Type Safety**: Use TypeScript interfaces for better type safety
7. **Memory Management**: Clear unused data from storage periodically

## TypeScript Support

Full TypeScript support with comprehensive type definitions:

```typescript
import type {
  DebounceOptions,
  BindInputOptions,
  StorageOptions,
  PerformanceTimer
} from '@nosto/search-js/utils';

const options: DebounceOptions = {
  leading: true,
  trailing: true,
  maxWait: 1000
};
```

## Browser Compatibility

The utils module supports all modern browsers and includes polyfills for:

- `Object.assign` (IE11+)
- `Array.from` (IE11+) 
- `localStorage` with fallbacks
- `performance` API with fallbacks
- `requestAnimationFrame` with fallbacks