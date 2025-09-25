# Search JS Library

Nosto Search JS is a comprehensive TypeScript library that provides powerful search functionality and UI components for building modern e-commerce search experiences. The library is built with modularity in mind, offering specialized packages for different aspects of search implementation.

## Key Features

- **🔍 Advanced Search**: Core search functionality with retry logic and result caching
- **💰 Currency Formatting**: Automatic price formatting based on Nosto settings
- **🖼️ Image Processing**: Smart thumbnail resizing with Shopify and Nosto support
- **⚛️ Preact Components**: Pre-built UI components for modern search interfaces
- **🛠️ Utilities**: Comprehensive helper functions for common development tasks
- **📱 Responsive Design**: Mobile-first components with accessibility support
- **🔄 Real-time Updates**: Live search results with autocomplete functionality

## Installation

Install the library using your preferred package manager:

```bash
# Using npm
npm install @nosto/search-js

# Using yarn
yarn add @nosto/search-js

# Using pnpm
pnpm add @nosto/search-js
```

## Quick Start

```typescript
import { search } from '@nosto/search-js/core';
import { priceDecorator } from '@nosto/search-js/currencies';
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';

// Basic search configuration
const searchResults = await search({
  query: 'shoes',
  size: 20,
  decorators: [
    priceDecorator({ currency: 'USD' }),
    nostoThumbnailDecorator({ width: 300, height: 300 })
  ]
});
```

## Architecture Overview

The Search JS library is organized into several specialized modules:

### Core Modules

- **[Core](core.md)**: Essential search functionality, caching, and decorators
- **[Currencies](currencies.md)**: Price formatting and currency handling
- **[Thumbnails](thumbnails.md)**: Image resizing and optimization
- **[Utils](utils.md)**: Helper functions and common utilities

### Preact Components

The library includes a comprehensive set of Preact components for building search UIs:

- **[Autocomplete](preact/autocomplete.md)**: Smart search suggestions and autocomplete
- **[Hooks](preact/hooks.md)**: React hooks for state management
- **[SERP](preact/serp.md)**: Search Engine Results Pages with pagination and sorting
- **[Common](preact/common.md)**: Shared components and utilities
- **[Category](preact/category.md)**: Category-based search and filtering
- **[Events](preact/events.md)**: Event handling and analytics
- **[Inject](preact/inject.md)**: Component injection and dynamic rendering
- **[Legacy](preact/legacy.md)**: Backward compatibility components

## TypeScript Support

The library is built with TypeScript and provides comprehensive type definitions out of the box. All modules export their types, making it easy to build type-safe applications.

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Resources

- [GitHub Repository](https://github.com/Nosto/search-js)
- [TypeDoc Documentation](https://nosto.github.io/search-js/)
- [Nosto Tech Docs](https://docs.nosto.com/techdocs/apis/frontend/oss/search-js)
- [NPM Package](https://www.npmjs.com/package/@nosto/search-js)

## Contributing

The Search JS library is developed and maintained by Nosto. For issues, feature requests, or contributions, please visit the [GitHub repository](https://github.com/Nosto/search-js).