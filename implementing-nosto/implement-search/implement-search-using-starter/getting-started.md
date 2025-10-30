# Getting Started

This guide will walk you through setting up and running the Search Templates Starter project on your local machine for development.

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

First, clone the Search Templates Starter repository to your local machine:

```sh
git clone https://github.com/nosto/search-templates-starter.git
cd search-templates-starter
```

### 2. Install dependencies

Install the required npm packages:

```sh
npm install
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

**Option 1: Environment Variable**
```bash
VITE_MERCHANT_ID=your-merchant-id npm run dev
```

**Option 2: Environment File**
Create a `.env` file in the root of the project:
```
VITE_MERCHANT_ID=your-merchant-id
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

### Understanding Development Modes

The Search Templates Starter supports multiple development modes to accommodate different workflows:

#### Native Mode
```bash
npm run dev:native
```
In this mode, the application renders as a standalone Preact app. This is useful for:
- Developing components in isolation
- Testing search functionality without a live site
- Rapid prototyping of new features

#### Injected Mode
```bash
npm run dev
```
This is the default mode where search components are injected into a live site using React Portals. To use this mode effectively:
1. Configure CSS selectors in `src/config.ts` to match your site's elements
2. Ensure your site is accessible for injection
3. Test on your actual store to see real integration

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
