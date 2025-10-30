# Project Structure

The Search Templates Starter is organized to promote scalability and maintainability.

```
/
├── src/
│   ├── components/     # Reusable Preact components
│   ├── contexts/       # React contexts for state management
│   ├── elements/       # Atomic UI elements
│   ├── entries/        # Entry points for different modes (native, injected)
│   ├── hooks/          # Custom hooks
│   ├── mapping/        # Data mapping and transformation logic
│   ├── plugins/        # Vite plugins
│   └── utils/          # Utility functions
├── test/               # Test files
│   └── e2e/            # End-to-end tests
├── mocks/              # Mock data for development and testing
├── docs/               # Documentation
└── ...                 # Configuration files
```

## Pros of this structure:

-   **Modularity:** Components are broken down into small, reusable pieces, making them easy to manage and test.
-   **Separation of Concerns:** The structure separates components, business logic, and configuration, making the codebase easier to understand and navigate.
-   **Scalability:** The organization is designed to scale with your project, allowing you to add new features and components without cluttering the codebase.
-   **Testability:** With dedicated directories for tests and mocks, writing and running tests is straightforward.
