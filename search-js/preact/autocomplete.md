# Preact Autocomplete

The autocomplete module provides smart search suggestion components that enhance user experience with real-time search results, product suggestions, and intelligent query completion. Built for high performance and accessibility.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';
import { SearchProvider } from '@nosto/search-js/preact/common';

const App = () => (
  <SearchProvider config={{ accountId: 'your-account-id' }}>
    <AutocompleteInput
      placeholder="Search for products..."
      onSelect={(suggestion) => {
        console.log('Selected:', suggestion);
        window.location.href = suggestion.url;
      }}
    />
  </SearchProvider>
);
```

## Components

### AutocompleteInput

The main autocomplete input component with intelligent suggestions.

```tsx
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';

<AutocompleteInput
  placeholder="Search products..."
  minLength={2}
  debounce={300}
  maxSuggestions={8}
  showCategories={true}
  showProducts={true}
  showQueries={true}
  onSelect={(suggestion) => handleSelection(suggestion)}
  onSearch={(query) => handleSearch(query)}
  className="custom-autocomplete"
  renderSuggestion={(suggestion) => (
    <CustomSuggestionItem suggestion={suggestion} />
  )}
/>
```

### AutocompleteSuggestions

Standalone suggestions dropdown component.

```tsx
import { AutocompleteSuggestions } from '@nosto/search-js/preact/autocomplete';

<AutocompleteSuggestions
  query="running shoes"
  visible={true}
  onSelect={(suggestion) => handleSelection(suggestion)}
  maxResults={10}
  groupBy="type"
  renderGroup={(group, suggestions) => (
    <div className="suggestion-group">
      <h3>{group}</h3>
      {suggestions.map(renderSuggestion)}
    </div>
  )}
/>
```

### VoiceSearchInput

Voice-enabled search input with speech recognition.

```tsx
import { VoiceSearchInput } from '@nosto/search-js/preact/autocomplete';

<VoiceSearchInput
  placeholder="Search or speak..."
  language="en-US"
  continuous={false}
  interimResults={true}
  onVoiceStart={() => setListening(true)}
  onVoiceEnd={() => setListening(false)}
  onVoiceResult={(transcript) => handleVoiceSearch(transcript)}
  renderVoiceIndicator={({ listening, supported }) => (
    <VoiceButton listening={listening} supported={supported} />
  )}
/>
```

## Key Features

### 🎯 Intelligent Suggestions

Multi-type suggestions including products, categories, and query completions.

```tsx
<AutocompleteInput
  suggestionTypes={[
    { type: 'products', limit: 4, boost: 1.2 },
    { type: 'categories', limit: 3, boost: 1.0 },
    { type: 'queries', limit: 3, boost: 0.8 }
  ]}
  renderSuggestion={(suggestion) => {
    switch (suggestion.type) {
      case 'product':
        return <ProductSuggestion product={suggestion} />;
      case 'category':
        return <CategorySuggestion category={suggestion} />;
      case 'query':
        return <QuerySuggestion query={suggestion} />;
    }
  }}
/>
```

### ⌨️ Keyboard Navigation

Full keyboard support with arrow keys, enter, and escape.

```tsx
<AutocompleteInput
  keyboardNavigation={true}
  highlightFirst={true}
  closeOnEscape={true}
  submitOnEnter={true}
  clearOnEscape={true}
  onKeyDown={(event, suggestion) => {
    if (event.key === 'Tab' && suggestion) {
      // Custom tab behavior
      handleTabCompletion(suggestion);
    }
  }}
/>
```

### 📱 Mobile Optimization

Touch-friendly interface with swipe gestures and mobile-specific features.

```tsx
<AutocompleteInput
  mobile={{
    touchOptimized: true,
    swipeToClose: true,
    hapticFeedback: true,
    fullscreen: true // On small screens
  }}
  renderMobileHeader={() => (
    <div className="mobile-search-header">
      <button onClick={closeMobileSearch}>Cancel</button>
    </div>
  )}
/>
```

### 🔍 Search Highlighting

Highlight matching terms in suggestions.

```tsx
<AutocompleteInput
  highlightMatches={true}
  highlightTag="mark"
  highlightClassName="search-highlight"
  renderSuggestion={(suggestion, query) => (
    <div className="suggestion">
      <span 
        dangerouslySetInnerHTML={{
          __html: highlightMatches(suggestion.text, query)
        }}
      />
    </div>
  )}
/>
```

## Advanced Usage

### Custom Suggestion Rendering

```tsx
const CustomAutocomplete = () => {
  const renderSuggestion = (suggestion) => {
    switch (suggestion.type) {
      case 'product':
        return (
          <div className="product-suggestion">
            <img src={suggestion.imageUrl} alt={suggestion.name} />
            <div className="product-info">
              <h4>{suggestion.name}</h4>
              <p className="price">{suggestion.formattedPrice}</p>
              <p className="category">{suggestion.category}</p>
            </div>
          </div>
        );
      
      case 'category':
        return (
          <div className="category-suggestion">
            <span className="category-icon">📁</span>
            <span>{suggestion.name}</span>
            <span className="count">({suggestion.productCount})</span>
          </div>
        );
      
      case 'query':
        return (
          <div className="query-suggestion">
            <span className="search-icon">🔍</span>
            <span>{suggestion.text}</span>
          </div>
        );
      
      default:
        return <div>{suggestion.text}</div>;
    }
  };

  return (
    <AutocompleteInput
      renderSuggestion={renderSuggestion}
      onSelect={(suggestion) => {
        if (suggestion.type === 'product') {
          window.location.href = suggestion.url;
        } else if (suggestion.type === 'category') {
          window.location.href = `/category/${suggestion.slug}`;
        } else {
          performSearch(suggestion.text);
        }
      }}
    />
  );
};
```

### Integration with Search Context

```tsx
import { useSearchContext } from '@nosto/search-js/preact/common';
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';

const SearchWithContext = () => {
  const { 
    search, 
    suggestions, 
    recentSearches, 
    popularSearches 
  } = useSearchContext();

  return (
    <AutocompleteInput
      onSearch={search}
      suggestions={suggestions}
      recentSearches={recentSearches}
      popularSearches={popularSearches}
      showRecent={true}
      showPopular={true}
      renderEmpty={() => (
        <div className="empty-suggestions">
          <h4>Popular Searches</h4>
          {popularSearches.map(term => (
            <button 
              key={term}
              onClick={() => search(term)}
              className="popular-search"
            >
              {term}
            </button>
          ))}
        </div>
      )}
    />
  );
};
```

### Custom Filtering and Sorting

```tsx
<AutocompleteInput
  filterSuggestions={(suggestions, query) => {
    // Custom filtering logic
    return suggestions
      .filter(s => s.relevanceScore > 0.5)
      .sort((a, b) => b.popularity - a.popularity);
  }}
  groupSuggestions={(suggestions) => {
    // Group by category
    return suggestions.reduce((groups, suggestion) => {
      const group = suggestion.category || 'Other';
      if (!groups[group]) groups[group] = [];
      groups[group].push(suggestion);
      return groups;
    }, {});
  }}
/>
```

## Configuration Options

### AutocompleteInput Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `placeholder` | `string` | "Search..." | Input placeholder text |
| `minLength` | `number` | 2 | Minimum query length |
| `debounce` | `number` | 300 | Debounce delay in ms |
| `maxSuggestions` | `number` | 8 | Maximum suggestions to show |
| `showCategories` | `boolean` | true | Show category suggestions |
| `showProducts` | `boolean` | true | Show product suggestions |
| `showQueries` | `boolean` | true | Show query suggestions |
| `autoFocus` | `boolean` | false | Auto-focus input on mount |
| `clearOnSelect` | `boolean` | false | Clear input after selection |
| `closeOnSelect` | `boolean` | true | Close dropdown after selection |

### Styling Props

| Prop | Type | Description |
|------|------|-------------|
| `className` | `string` | CSS class for container |
| `inputClassName` | `string` | CSS class for input element |
| `dropdownClassName` | `string` | CSS class for dropdown |
| `suggestionClassName` | `string` | CSS class for suggestions |
| `highlightClassName` | `string` | CSS class for highlighted text |

### Event Handlers

| Prop | Type | Description |
|------|------|-------------|
| `onSearch` | `(query: string) => void` | Called when search is triggered |
| `onSelect` | `(suggestion: Suggestion) => void` | Called when suggestion is selected |
| `onFocus` | `() => void` | Called when input receives focus |
| `onBlur` | `() => void` | Called when input loses focus |
| `onChange` | `(value: string) => void` | Called when input value changes |

## API Reference

### Suggestion Types

```typescript
interface ProductSuggestion {
  type: 'product';
  id: string;
  name: string;
  url: string;
  imageUrl?: string;
  price?: number;
  formattedPrice?: string;
  category?: string;
  brand?: string;
  relevanceScore: number;
}

interface CategorySuggestion {
  type: 'category';
  id: string;
  name: string;
  slug: string;
  url: string;
  productCount?: number;
  relevanceScore: number;
}

interface QuerySuggestion {
  type: 'query';
  text: string;
  popularity?: number;
  relevanceScore: number;
}

type Suggestion = ProductSuggestion | CategorySuggestion | QuerySuggestion;
```

### AutocompleteConfig

```typescript
interface AutocompleteConfig {
  accountId: string;
  apiUrl?: string;
  debounce?: number;
  minLength?: number;
  maxSuggestions?: number;
  enableVoice?: boolean;
  enableHighlighting?: boolean;
  languages?: string[];
}
```

## Styling and Theming

### CSS Custom Properties

```css
:root {
  --autocomplete-background: #ffffff;
  --autocomplete-border: #e1e5e9;
  --autocomplete-border-radius: 4px;
  --autocomplete-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  --autocomplete-max-height: 400px;
  
  --suggestion-padding: 12px 16px;
  --suggestion-hover-background: #f8f9fa;
  --suggestion-active-background: #e9ecef;
  --suggestion-text-color: #212529;
  --suggestion-meta-color: #6c757d;
  
  --highlight-background: #fff3cd;
  --highlight-color: #856404;
}
```

### Component Classes

```css
.nosto-autocomplete {
  /* Container styles */
}

.nosto-autocomplete__input {
  /* Input field styles */
}

.nosto-autocomplete__dropdown {
  /* Dropdown container styles */
}

.nosto-autocomplete__suggestion {
  /* Individual suggestion styles */
}

.nosto-autocomplete__suggestion--highlighted {
  /* Highlighted suggestion styles */
}

.nosto-autocomplete__suggestion--selected {
  /* Selected suggestion styles */
}

.nosto-autocomplete__group {
  /* Suggestion group styles */
}

.nosto-autocomplete__empty {
  /* Empty state styles */
}
```

## Accessibility

The autocomplete components follow WCAG 2.1 AA guidelines:

- **Keyboard Navigation**: Full keyboard support with arrow keys
- **Screen Reader Support**: Proper ARIA labels and announcements
- **Focus Management**: Logical focus order and visible focus indicators
- **High Contrast**: Supports high contrast modes
- **Reduced Motion**: Respects prefers-reduced-motion settings

```tsx
<AutocompleteInput
  ariaLabel="Search for products"
  ariaDescribedBy="search-help"
  announceResults={true}
  announceSelection={true}
  liveRegion={true}
/>
```

## Performance Optimization

### Debouncing and Caching

```tsx
<AutocompleteInput
  debounce={300}
  cache={{
    enabled: true,
    ttl: 300000, // 5 minutes
    maxSize: 100
  }}
  prefetch={{
    enabled: true,
    popularQueries: ['shoes', 'shirts', 'jeans'],
    prefetchDelay: 1000
  }}
/>
```

### Virtual Scrolling

For large suggestion lists:

```tsx
<AutocompleteInput
  virtualScrolling={{
    enabled: true,
    itemHeight: 60,
    maxHeight: 300,
    overscan: 5
  }}
/>
```

## Testing

```tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/preact';
import { AutocompleteInput } from '@nosto/search-js/preact/autocomplete';

test('should show suggestions when typing', async () => {
  render(<AutocompleteInput />);
  
  const input = screen.getByRole('textbox');
  fireEvent.input(input, { target: { value: 'test' } });
  
  await waitFor(() => {
    expect(screen.getByRole('listbox')).toBeInTheDocument();
  });
});

test('should select suggestion on click', async () => {
  const onSelect = jest.fn();
  render(<AutocompleteInput onSelect={onSelect} />);
  
  const input = screen.getByRole('textbox');
  fireEvent.input(input, { target: { value: 'test' } });
  
  await waitFor(() => {
    const suggestion = screen.getByRole('option');
    fireEvent.click(suggestion);
    expect(onSelect).toHaveBeenCalled();
  });
});
```