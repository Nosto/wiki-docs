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

- **AGENTS.md Standard** - Follow the [agents.md](https://agents.md/) standardized pattern for providing AI coding agents with project-specific context, build commands, code style guidelines, and testing instructions
- **Copilot Instructions** - Pre-configured GitHub Copilot instructions are included in the repository and should be customized for your specific use case
- **README patterns** - Follow the established documentation structure for consistency

> **Tip:** Consider creating an `AGENTS.md` file in your project root following the [agents.md standard](https://agents.md/) to provide consistent context for all AI coding tools.

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

### Component Modifications

**Example: Replace FilterSidebar with FilterTopbar**
```
Replace the existing FilterSidebar component with a FilterTopbar component that displays filters horizontally at the top of the search results.

Update the layout in Serp component to render FilterTopbar above the products grid instead of FilterSidebar in the sidebar. Maintain all existing filter functionality while adapting the responsive design for horizontal layout.
```

**Example: Replace Pills with Checkboxes in Filters**
```
Replace the Pill components used in FilterSidebar with checkbox inputs for better accessibility and mobile usability.

Update the filter display to show checkboxes with labels instead of pill-style buttons, while maintaining the same filter state management and visual feedback for selected filters.
```

### Styling Changes

**Example: Replace CSS Modules with Tailwind**
```
Convert the Button component from CSS modules to Tailwind CSS classes.

Replace the current Button.module.css imports and className usage with Tailwind utility classes, maintaining the same visual appearance and hover states.
```

### Search Functionality

**Example: Add Voice Search to Autocomplete**
```
Integrate the existing SpeechToTextButton component into the main search input in the Serp component.

Position the voice search button inside the search input field and ensure it works consistently across both autocomplete and main search interfaces.
```

**Example: Replace Infinite Scroll with Load More Button**
```
Replace the current pagination in search results with a "Load More" button that appends new results to the existing list.

Update the Pagination component to show a centered button instead of page numbers, and modify the search state to accumulate results rather than replace them.
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
