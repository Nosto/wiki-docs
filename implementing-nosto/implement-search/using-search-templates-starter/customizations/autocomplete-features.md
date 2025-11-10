# Autocomplete features

## Enabling Autocomplete Features

This document explains how to enable and configure various features for the autocomplete component, including category suggestions and trending searches.

To enable these features, you only need to modify the `withAutocompleteDefaults` function in `src/config.ts` to request the necessary data. The UI components in `src/components/Autocomplete/Results/` are already set up to render this data once it's fetched.

***

### Enabling Category Suggestions

You can enhance the autocomplete experience by including category suggestions alongside product and keyword results.

#### Configuration

To enable this feature, add a `categories` object to the query returned by the `withAutocompleteDefaults` function.

**File to edit:** `src/config.ts`

```typescript
// src/config.ts

function withAutocompleteDefaults(query: SearchQuery) {
  return {
    ...query,
    // ... other properties
    categories: {
      fields: ["name", "url"],
      size: 3 // The maximum number of category suggestions to fetch
    }
  } satisfies SearchQuery
}
```

#### How It Works

By adding the `categories` object to the query, you are instructing `@nosto/search-js` to fetch category suggestions. The `useResponse` hook within the `Results` component (`src/components/Autocomplete/Results/Results.tsx`) will then receive this data.

The `Results` component, in turn, passes the category data to the `Categories` component (`src/components/Autocomplete/Results/Categories.tsx`), which is responsible for rendering the suggestions. No additional rendering configuration is needed.

***

### Enabling Popular Searches

Displaying popular searches can help guide users and improve product discovery. This feature shows suggestions based on what other shoppers are searching for.

#### Configuration

To enable this feature, add a `popularSearches` object to the query returned by the `withAutocompleteDefaults` function.

**File to edit:** `src/config.ts`

```typescript
// src/config.ts

function withAutocompleteDefaults(query: SearchQuery) {
  return {
    ...query,
    // ... other properties
    popularSearches: {
      fields: ["query"],
      size: 3 // The maximum number of trending searches to fetch
    }
  } satisfies SearchQuery
}
```

#### How It Works

Similar to category suggestions, adding the `popularSearches` object to the query will cause the data to be fetched. The `useResponse` hook in the `Results` component will receive the data and pass it to the `PopularSearches` component (`src/components/Autocomplete/Results/PopularSearches.tsx`), which handles the rendering automatically.
