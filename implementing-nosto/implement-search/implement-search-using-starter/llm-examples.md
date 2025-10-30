# Utilizing LLMs for Development

Large Language Models (LLMs) like GitHub Copilot, ChatGPT, and Claude can significantly accelerate development with the Search Templates Starter. This guide provides proven prompts and strategies for common development tasks.

## Why use LLMs with Search Templates Starter?

The Search Templates Starter's well-structured codebase and modern tooling make it ideal for AI assistance:

-   **Consistent patterns** - The organized project structure helps LLMs understand context
-   **TypeScript support** - Type information provides better AI suggestions and error detection
-   **Component-based architecture** - Clear boundaries make it easier to generate focused code
-   **Testing infrastructure** - LLMs can generate tests alongside implementation code

## Effective Prompting Strategies

### Follow Standard Development Patterns
The Search Templates Starter includes standard development patterns and documentation that LLMs can leverage:

- **AGENTS.md** - Contains specific prompting patterns and conventions for the project
- **Copilot Instructions** - Pre-configured GitHub Copilot instructions are included in the repository and should be customized for your specific use case
- **README patterns** - Follow the established documentation structure for consistency

### Include Context
Always provide relevant context about the project structure and existing patterns:

```
"In this Preact-based Search Templates Starter project that uses TypeScript, Vite, and follows the component structure in src/components/, create a..."
```

### Specify File Locations
Be explicit about where new code should be placed:

```
"Create the component in src/components/ProductQuickView/ and include a corresponding Storybook story in the same directory."
```

### Reference Existing Patterns
Point to existing code as examples:

```
"Following the same pattern as the existing SearchResults component, create a..."
```

## Common Development Tasks

### Adding New Components

**Prompt Template:**
```
Create a new [ComponentName] component for the Search Templates Starter project. 

Requirements:
- Place in src/components/[ComponentName]/
- Use TypeScript and Preact
- Follow the existing component patterns
- Include PropTypes/interface definitions
- Create a Storybook story
- Add basic CSS modules for styling
- Include unit tests with React Testing Library

The component should [specific functionality requirements].

Reference the existing [SimilarComponent] component for patterns.
```

**Example: Product Quick View**
```
Create a new ProductQuickView component for the Search Templates Starter project.

Requirements:
- Place in src/components/ProductQuickView/
- Use TypeScript and Preact
- Follow the existing component patterns
- Include PropTypes/interface definitions  
- Create a Storybook story
- Add basic CSS modules for styling
- Include unit tests with React Testing Library

The component should display product details in a modal overlay when triggered, showing product images, name, price, description, and an "Add to Cart" button. Use the existing Portal element for modal rendering.

Reference the existing SearchResults component for patterns and the Portal element usage.
```

### Styling and Theme Changes

**Example: Migrating to Tailwind CSS**
```
Replace the existing CSS modules structure in the Search Templates Starter with Tailwind CSS.

Steps needed:
1. Install and configure Tailwind CSS with Vite
2. Update vite.config.ts to include Tailwind
3. Create a tailwind.config.js with appropriate configuration
4. Replace all CSS module imports and classes with Tailwind utility classes
5. Remove the original CSS module files
6. Update Storybook configuration to support Tailwind
7. Ensure the existing component functionality remains unchanged

Start with the Button element in src/elements/Button/ as an example, then apply to all components.
```

### API Integration

**Example: Adding Search Filters**
```
Add advanced filtering functionality to the Search Templates Starter.

Requirements:
- Create a FilterPanel component in src/components/FilterPanel/
- Add filter state management to src/contexts/SearchContext.tsx
- Create custom hooks in src/hooks/useFilters.ts for filter logic
- Add filter mapping functions in src/mapping/filterMapping.ts
- Update the search API calls to include filter parameters
- Add TypeScript interfaces for filter data structures
- Include comprehensive tests for filter functionality

The filters should support:
- Price range filtering
- Category selection
- Brand filtering  
- Rating filtering
- Custom attribute filtering

Reference the existing search implementation patterns and Nosto Search API documentation.
```

### Testing

**Example: Adding E2E Tests**
```
Create comprehensive end-to-end tests for the Search Templates Starter using Playwright.

Create tests in test/e2e/ for:
- Basic search functionality
- Autocomplete behavior
- Filter interactions
- Pagination
- Mobile responsive behavior
- Performance testing

Tests should:
- Use Page Object Model pattern
- Include setup for different merchant configurations
- Test against both mocked and live Nosto data
- Include accessibility testing
- Generate test reports

Reference the existing test structure and follow Playwright best practices.
```

### Performance Optimization

**Example: Bundle Optimization**
```
Optimize the Search Templates Starter build for production performance.

Implement:
- Code splitting for components and routes
- Lazy loading for non-critical components
- Tree shaking optimization in vite.config.ts
- Asset optimization and compression
- Bundle analysis and size monitoring
- Performance monitoring setup

Focus on:
- Reducing initial bundle size
- Improving Time to Interactive (TTI)
- Optimizing Core Web Vitals
- Maintaining development experience

Provide before/after bundle analysis and performance metrics.
```

## Advanced Use Cases

### Custom Hook Development

**Example: Search Analytics Hook**
```
Create a custom hook useSearchAnalytics in src/hooks/ that:

- Tracks search events and user interactions
- Integrates with Google Analytics and Nosto Analytics
- Provides search performance metrics
- Handles event batching and error handling
- Includes TypeScript interfaces for analytics data
- Follows the existing hook patterns in the project

The hook should work with the existing SearchContext and be easily testable.
```

### Configuration Management

**Example: Multi-Environment Config**
```
Enhance the configuration system in src/config.ts to support multiple environments.

Requirements:
- Support development, staging, and production configurations
- Environment-specific API endpoints and settings
- Type-safe configuration with TypeScript
- Runtime configuration validation
- Easy switching between configurations
- Integration with Vite environment variables
- Documentation for adding new configuration options

Follow the existing configuration patterns and maintain backward compatibility.
```

## Best Practices for LLM-Assisted Development

### Code Review
Always review LLM-generated code for:
- Adherence to project patterns and conventions
- TypeScript type safety
- Security considerations
- Performance implications
- Test coverage completeness

### Iterative Refinement
Start with basic prompts and refine:
1. Get a working implementation
2. Ask for improvements and optimizations
3. Add error handling and edge cases
4. Enhance with additional features
5. Optimize for performance and maintainability

### Combine with Human Expertise
Use LLMs to:
- Generate boilerplate code quickly
- Explore different implementation approaches
- Create comprehensive test suites
- Document complex functionality

But rely on human judgment for:
- Architecture decisions
- Security considerations
- Performance trade-offs
- User experience design

## Troubleshooting LLM Issues

### Common Problems

**Generated code doesn't follow project patterns:**
- Include more specific context about existing patterns
- Reference specific files as examples
- Provide the project structure in your prompt

**TypeScript errors in generated code:**
- Ask the LLM to review and fix TypeScript errors
- Provide the exact error messages for targeted fixes
- Include relevant type definitions in your prompt

**Tests fail or are incomplete:**
- Request test coverage for specific scenarios
- Ask for tests that follow existing test patterns
- Include example test files for reference

**Generated code lacks optimization:**
- Ask specifically for performance considerations
- Request code review focusing on optimization
- Include performance requirements in your initial prompt

By following these patterns and examples, you can significantly accelerate your development workflow while maintaining code quality and project consistency.
