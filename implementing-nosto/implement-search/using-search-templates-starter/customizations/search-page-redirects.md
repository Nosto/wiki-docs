# Search page redirects

## Autocomplete Search Submission and Redirects

This document explains how submitting a search from the autocomplete component navigates the user to the Search Engine Results Page (SERP).

***

### Overview

When a user selects a suggestion or submits a query from the autocomplete dropdown, the application's behavior depends on the user's current location. The goal is to ensure the user always ends up on the main SERP to see the full results.

This logic is primarily handled within the `onSubmit` function in the `Search` component.

**File to inspect:** `src/components/Search/Search.tsx`

***

### How It Works

The `onSubmit` function checks the current page's URL (`window.location.pathname`) to decide whether to perform an in-place search update or a full-page redirect.

#### Scenario 1: User is Already on the SERP

If the user is already on a page that includes `/search` in its path, submitting a new search will **not** cause a full-page redirect.

* The `newSearch({ query })` action is dispatched.
* `@nosto/search-js` fetches the new results.
* The components on the SERP update in place to display the new results.

This provides a fast and smooth experience when refining a search.

#### Scenario 2: User is on Another Page (e.g., Homepage)

If the user performs a search from any page that is **not** the SERP (e.g., the homepage, a content page), the application will perform a full-page redirect to the SERP.

* The browser is redirected to `/search?q=<your-query>`.
* The SERP then loads, reads the `q` parameter from the URL, and automatically fetches and displays the results for that query.

This ensures a consistent experience, where a search always leads the user to the dedicated, fully-featured search results page.

#### Implementation Example

Here is a simplified look at the logic inside `src/components/Search/Search.tsx`:

```tsx
// src/components/Search/Search.tsx

export default function Search() {
  const { newSearch } = useActions();

  const onSubmit = (query: string) => {
    // If we are already on the search page, just update the results
    if (window.location.pathname.includes("/search")) {
      newSearch({ query });
    } else {
      // Otherwise, redirect to the main search page
      window.location.href = `/search?q=${encodeURIComponent(query)}`;
    }
  };

  // ... component JSX that uses onSubmit
}
```
