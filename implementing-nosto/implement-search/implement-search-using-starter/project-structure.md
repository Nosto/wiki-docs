# Project Structure

The Search Templates Starter is carefully organized to promote scalability, maintainability, and developer productivity. This structure follows modern React/Preact best practices and provides clear separation of concerns.

## Why this structure?

This project organization is designed to:

-   **Scale with your project** - Easy to add new features without cluttering the codebase
-   **Promote reusability** - Components and utilities can be easily shared and tested
-   **Maintain clarity** - Clear separation between different types of code and concerns
-   **Support testing** - Dedicated structure for tests and mocks makes testing straightforward
-   **Enable collaboration** - Team members can quickly understand and contribute to different areas

## Directory Overview

```
search-templates-starter/
├── src/                    # Main source code directory
│   ├── components/         # Reusable Preact components
│   ├── contexts/           # React contexts for state management
│   ├── elements/           # Atomic UI elements and building blocks
│   ├── entries/            # Entry points for different modes (native, injected)
│   ├── hooks/              # Custom React hooks
│   ├── mapping/            # Data mapping and transformation logic
│   ├── plugins/            # Vite plugins for build customization
│   ├── utils/              # Utility functions and helpers
│   └── config.ts           # Application configuration
├── test/                   # Test files and test utilities
│   ├── e2e/                # End-to-end tests with Playwright
│   └── unit/               # Unit and integration tests
├── mocks/                  # Mock data for development and testing
├── docs/                   # Project documentation
├── .storybook/             # Storybook configuration
├── public/                 # Static assets
└── dist/                   # Build output (generated)
```

## Core Directories Explained

### `/src/components/`
**Purpose**: Houses reusable UI components that represent complete features or sections.

**Examples**:
- `SearchResults/` - Complete search results display
- `Autocomplete/` - Autocomplete dropdown functionality  
- `FilterPanel/` - Product filtering interface
- `Pagination/` - Navigation between result pages

**Best Practices**:
- Each component should have its own directory
- Include component file, styles, tests, and Storybook stories
- Components should be self-contained and reusable

### `/src/elements/`
**Purpose**: Contains atomic UI elements - the smallest building blocks of your interface.

**Examples**:
- `Button/` - Basic button component
- `Input/` - Form input elements
- `Card/` - Product card layout
- `Portal/` - Portal component for modals

**Guidelines**:
- Keep elements simple and focused on single responsibility
- Highly reusable across different components
- Minimal business logic, mostly presentation

### `/src/contexts/`
**Purpose**: React contexts for sharing state across the application.

**Key Files**:
- `SearchContext.tsx` - Global search state and actions
- `ConfigContext.tsx` - Application configuration
- `ThemeContext.tsx` - Theming and styling state

**Usage**:
- Provides global state management without heavy libraries
- Makes data available to deeply nested components
- Centralizes state logic for easier testing

### `/src/hooks/`
**Purpose**: Custom React hooks that encapsulate reusable logic.

**Examples**:
- `useSearch.ts` - Search functionality and state
- `useDebounce.ts` - Debouncing user input
- `useLocalStorage.ts` - Browser storage integration
- `usePagination.ts` - Pagination logic

**Benefits**:
- Reusable stateful logic across components
- Easier testing of complex logic
- Clean separation of concerns

### `/src/entries/`
**Purpose**: Entry points for different application modes.

**Files**:
- `native.tsx` - Standalone application entry
- `injected.tsx` - Injection into existing sites
- `storybook.tsx` - Storybook-specific setup

**Why Separate**:
- Different modes may require different initialization
- Allows optimization for specific use cases
- Cleaner build configuration

### `/src/mapping/`
**Purpose**: Data transformation and mapping between Nosto API responses and component props.

**Responsibilities**:
- Transform API data to component-friendly formats
- Handle data validation and type safety
- Normalize data from different sources
- Apply business logic to raw data

### `/src/utils/`
**Purpose**: Pure utility functions that don't depend on React or component state.

**Examples**:
- `formatPrice.ts` - Price formatting logic
- `searchHelpers.ts` - Search query manipulation
- `validation.ts` - Input validation functions
- `constants.ts` - Application constants

## Configuration and Build

### Key Configuration Files

**`src/config.ts`**
Central configuration for:
- API endpoints and merchant settings
- CSS selectors for site integration
- Feature flags and behavior toggles
- Default search parameters

**`vite.config.ts`**
Build configuration including:
- Plugin setup and customization
- Build optimization settings
- Development server configuration
- Environment variable handling

**`.storybook/main.ts`**
Storybook configuration for:
- Component story discovery
- Addon configuration
- Build settings for isolated development

## Testing Structure

### `/test/unit/`
Unit and integration tests using Vitest:
- Component testing with React Testing Library
- Hook testing with custom test utilities
- Utility function testing
- Mock setup and test helpers

### `/test/e2e/`
End-to-end tests with Playwright:
- User workflow testing
- Cross-browser compatibility
- Performance testing
- Integration with live Nosto data

## Development Workflow Integration

### Local Development
- `src/` contains all active development code
- Hot reloading watches source files
- TypeScript compilation happens automatically
- Storybook runs parallel for component development

### Build Process
1. TypeScript compilation with type checking
2. Vite bundling and optimization  
3. Asset processing and compression
4. Generation of build artifacts in `dist/`

### Deployment
- CLI tools upload `dist/` contents to Nosto
- Source maps for debugging in production
- Automatic cache busting for updates
