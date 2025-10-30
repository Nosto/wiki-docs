# Getting Started

## Why use the Search Templates Starter?

The Search Templates Starter provides a complete development environment for building custom search experiences with modern tools and practices. It's designed for developers who need:

-   **Full control** over the search implementation and codebase
-   **Modern development workflow** with TypeScript, testing, and component development
-   **Local development environment** that integrates with your existing tools and processes
-   **Version control** and collaborative development capabilities

## Prerequisites

Before you begin, ensure you have the following installed on your system:

-   **Node.js** (v22 or higher) - [Download from nodejs.org](https://nodejs.org/)
-   **npm** (comes with Node.js) or **yarn** as your package manager
-   **Git** for version control
-   A **Nosto account** with Search module enabled
-   Basic familiarity with **React/Preact** and modern JavaScript development

## Installation

### 1. Clone the repository

First, clone the Search Templates Starter repository to your local machine. You can do this using the command line or via the GitHub UI:

**Option 1: Command line**
```sh
git clone https://github.com/nosto/search-templates-starter.git
cd search-templates-starter
```

**Option 2: GitHub UI**
1. Go to [Search Templates Starter on GitHub](https://github.com/nosto/search-templates-starter)
2. Click the **Code** button and select **Download ZIP** or **Open with GitHub Desktop**
3. Extract the ZIP file or clone the repository using your preferred Git client

```sh
git clone https://github.com/nosto/search-templates-starter.git
cd search-templates-starter
```

### 2. Install dependencies

Install the required npm packages:

```sh
npm ci
```

This will install all necessary dependencies including:
- Preact for the UI framework
- Vite for build tooling and development server
- TypeScript for type safety
- Vitest for testing
- Storybook for component development

## Configuration

### Environment Setup

The Search Templates Starter requires your Nosto merchant ID to connect to your search data. You can provide this in two ways:

**Option 1: Environment File (Recommended)**
Create a `.env` file in the root of the project:
```
VITE_MERCHANT_ID=your-merchant-id
```

**Option 2: Environment Variable**
```bash
VITE_MERCHANT_ID=your-merchant-id npm run dev
```

> **Finding your Merchant ID:** You can find your merchant ID in the Nosto Admin UI under Account Settings, or it's typically in the format `platform-storeid` (e.g., `shopify-12345678`).

### Development Configuration

The starter includes a configuration file at `src/config.ts` where you can customize:
- CSS selectors for integration with your site
- Search behavior and settings
- Component rendering options

## Local Development

### Starting the Development Server

To start the local development server:

```bash
npm run dev
```

This will launch the application at `http://localhost:8000` with hot reloading enabled. Any changes you make to the source code will be automatically reflected in the browser.

> **Important:** The local development server runs independently of your shop's styles. To see how your components will look with your shop's styling, you can:
> - Modify `index.html` to reference your shop's CSS files
> - Use the `nosto-cli watch` workflow for live deployment testing
> - Test directly on your shop's staging environment

### Understanding Development Modes

Search Templates Starter may operate in three modes: Injected, Native and Mocked. For a typical store setup, you will most likely be using the Injected mode, as it is designed to integrate with any store page. Native mode is useful if you want to develop your store from the ground up using Search Templates Starter, and Mocked is primarily used for development and Storybook.

#### Injected Mode (Default)
```bash
npm run dev
```
In this mode, the components are rendered into the page using [React Portals](https://react.dev/reference/react-dom/createPortal), targeting the elements you define with CSS selectors in `src/config.tsx`. Note that without correct selectors, the components will not appear in the page at all. After the injection step, the rest of the application behaves nearly the same as it would in native mode.

> **Entry point:** `src/entries/injected.tsx`

To use this mode effectively:
1. Configure CSS selectors in `src/config.ts` to match your site's elements
2. Ensure your site is accessible for injection
3. Test on your actual store to see real integration

#### Native Mode
```bash
npm run dev:native
```
In this mode, the Starter behaves like a standard React/Preact app but still uses React Portals with a single injection point (`#app`). It creates the component tree and renders both search and results components together based on the page type (search results or category page). This is useful for:
- Developing your store from the ground up using Search Templates Starter
- Testing search functionality without existing page constraints
- Rapid prototyping of new features

> **Entry point:** `src/entries/native.tsx`

#### Mocked Mode
Used automatically in Storybook and testing environments where components render with mock data for consistent development and testing.

## Development Tools

### Storybook for Component Development

Storybook provides an isolated environment for developing and testing individual components:

```sh
npm run storybook
```

This opens Storybook in your browser where you can:
- View all available components
- Test different component states and props
- Develop components without needing a full application context
- Create and maintain component documentation

### Essential Development Commands

Here are the key commands for your development workflow:

#### Testing
```bash
# Run unit and integration tests
npm run test

# Run tests in watch mode during development
npm run test:watch

# Run end-to-end tests with Playwright
npm run test:e2e
```

#### Code Quality
```bash
# Check for linting errors and style issues
npm run lint

# Automatically fix linting issues where possible
npm run lint:fix

# Check for TypeScript compilation errors
npm run typecheck
```

#### Building
```bash
# Create a production build
npm run build

# Preview the production build locally
npm run preview
```

## Next Steps

Once you have the development environment running, you can:

1. **Explore the project structure** - See [Project Structure](project-structure.md) for detailed information
2. **Use the Nosto CLI** - See [Using the Nosto CLI](nosto-cli.md) for deployment workflows
3. **Leverage AI assistance** - See [LLM Examples](llm-examples.md) for development productivity tips

## Troubleshooting

### Common Issues

**Port already in use:**
If port 8000 is already in use, Vite will automatically try the next available port. You can also specify a custom port:
```bash
npm run dev -- --port 3000
```

**Missing merchant ID:**
Ensure your `VITE_MERCHANT_ID` environment variable is set correctly. The application cannot connect to Nosto's search API without it.

**Build errors:**
Run `npm run typecheck` to identify TypeScript errors that might be causing build issues.

For additional support, refer to the [Nosto documentation](https://docs.nosto.com/) or contact Nosto support.
