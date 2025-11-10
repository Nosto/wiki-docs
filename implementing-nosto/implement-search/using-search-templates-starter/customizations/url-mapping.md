# URL Mapping

### Search Results Page URL Management

When a search is performed (either by submitting the search form or clicking a non-redirect keyword), the user is taken to the search results page. The state of the search (query, filters, pagination, etc.) is reflected in the URL's query parameters.

#### URL Structure

The search results page URL is managed by a set of utility functions in `src/mapping/url/`. The URL is constructed with the following parameters:

* `q`: The search query.
* `p`: The current page number (omitted for the first page).
* `size`: The number of results per page.
* `filter.*`: Applied filters (e.g., `filter.brand=Nike`).
* `sort`: The selected sorting option.

#### How It Works

1. **State Management**: The `SearchQueryHandler` component (`src/components/SearchQueryHandler/SearchQueryHandler.tsx`) is responsible for synchronizing the application's search state with the URL.
2. **URL Updates**: It uses the `updateUrl` function (`src/mapping/url/updateUrl.ts`) to serialize the current search state into URL parameters and update the browser's history using `window.history.replaceState`.
3. **Initial State**: On page load, the `getCurrentUrlState` function (`src/mapping/url/getCurrentUrlState.ts`) deserializes the parameters from the URL to initialize the search state. This ensures that a user can share a URL, and it will load the same search results.

#### Example

A search for "shoes" on the second page with a filter for the brand "Nike" would result in a URL like this:

`/search?q=shoes&p=2&filter.brand=Nike`
