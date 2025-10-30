# Project Structure

The Search Templates Starter follows a clear, organized structure designed for scalability and maintainability.

## Directory Overview

```
search-templates-starter/
├── src/                    # Main source code
│   ├── components/         # Complete UI components  
│   ├── elements/           # Basic UI building blocks
│   ├── entries/            # Application entry points
│   ├── contexts/           # React contexts for state
│   ├── hooks/              # Custom React hooks
│   ├── mapping/            # Data transformation logic
│   ├── plugins/            # Vite build plugins
│   ├── utils/              # Utility functions
│   ├── config.ts           # Application configuration
│   └── variable.css        # CSS custom properties
├── test/                   # Testing files
│   ├── e2e/                # End-to-end tests (Playwright)
│   └── mocks/              # Mock data for testing
├── mocks/                  # Mock data for development
├── .storybook/             # Storybook configuration
└── build/                  # Build output (generated)
```

## Key Directories

### Components vs Elements

**`/src/components/`** - Complete features and UI sections:
- `Autocomplete/` - Search suggestions functionality
- `FilterSidebar/` - Product filtering interface  
- `Pagination/` - Navigation between result pages
- `Products/` - Product grid display
- `Search/` - Search input and form

**`/src/elements/`** - Basic UI building blocks:
- `Button/` - Button component
- `Checkbox/` - Form checkbox
- `Icon/` - Icon system
- `Portal/` - Portal for modals and overlays

### Application Structure

**`/src/entries/`** - Different app modes:
- `native.tsx` - Standalone application
- `injected.tsx` - Injection into existing sites

**`/src/contexts/`** - State management:
- React contexts for sharing state across components

**`/src/hooks/`** - Reusable logic:
- Custom hooks for search functionality and UI interactions

**`/src/mapping/`** - Data handling:
- Transform API responses to component-friendly formats
- URL state management
- Tagging integration

## Configuration

**`src/config.ts`** - Central configuration for:
- API settings and merchant configuration
- CSS selectors for site integration  
- Default search parameters
- Hit decorators (thumbnails, pricing)

**`vite.config.ts`** - Build configuration
**`.storybook/`** - Component development environment
**`nosto.config.ts`** - Nosto CLI integration
