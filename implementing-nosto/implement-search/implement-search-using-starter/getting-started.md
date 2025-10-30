# Getting Started

This guide will walk you through setting up and running the Search Templates Starter project on your local machine for development.

## Prerequisites

Ensure you have the following installed on your system:

-   Node.js (v22 or higher)
-   npm

## Installation

1.  **Clone the repository:**

    ```sh
    git clone https://github.com/nosto/search-templates-starter.git
    cd search-templates-starter
    ```

2.  **Install dependencies:**

    ```sh
    npm install
    ```

## Local Development

To start the local development server, run the following command:

```bash
VITE_MERCHANT_ID=your-merchant-id npm run dev
```

This will launch the application at `http://localhost:8000`. You can also provide the merchant ID via a `.env` file in the root of the project:

```
VITE_MERCHANT_ID=your-merchant-id
```

### Development Modes

The starter template can run in three different modes:

-   **Native Mode:** In this mode, the application renders as a standard Preact app. This is useful for developing components in isolation. Use `npm run dev:native` to run in this mode.
-   **Injected Mode:** This mode injects the search components into a live site using React Portals. You must configure the CSS selectors in `src/config.ts` to match the target elements on your page.
-   **Mocked Mode:** This mode is used for Storybook and testing, where components are rendered with mock data.

### Storybook

To work on components in isolation, you can use Storybook:

```sh
npm run storybook
```

This will open Storybook in your browser, where you can view and interact with all the available components.

### Useful Development Commands

Here are some other useful commands for local development:

-   `npm run test`: Run unit and integration tests with Vitest.
-   `npm run test:e2e`: Run end-to-end tests with Playwright.
-   `npm run lint`: Check the code for linting errors.
-   `npm run typecheck`: Check for TypeScript errors.
