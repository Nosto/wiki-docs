# Using Search Templates Starter

The Search Templates Starter is a Preact-based starter template that provides a complete development environment for building custom search experiences with Nosto.

### Prerequisites

Search Templates Starter is aimed at developers comfortable with:

* **TypeScript** — the codebase is TypeScript-first throughout
* **Git** — version control is central to the workflow
* **NPM-based tooling** — dependency management, scripts, local dev servers
* **React or Preact** — components follow standard React patterns

**Agentic coding** (using LLM tools such as Copilot, Codex or Claude) is optional but particularly useful here. Unlike the legacy VSCode web-based Search Templates, the Search Templates Starter is built with an assumption that AI tools are a staple of modern development. We recommend relying on LLMs to give you a better starting point before finalizing the feature development manually. For example, if you want to convert the starter to a different styling approach such as Tailwind, or swap out individual components for ones that better fit your stack, an LLM is useful. See [LLM Examples](llm-examples.md) for practical guidance.

### Why use Search Templates Starter?

Search Templates Starter allows you to build fully custom search implementations with modern development practices. You get:

* **Full source code control** with Git version management
* **Modern development tooling** including TypeScript, Vitest, and Storybook
* **Local development environment** with hot reloading and debugging
* **Component library** with pre-built, customizable search components
* **Complete flexibility** to modify any aspect of the search experience

For any serious development work, Search Templates Starter is the recommended choice. It gives you the tools and workflow you would expect from a modern frontend project, and it is the approach Nosto actively develops and supports.

### How it works (new to Nosto?)

If you have not worked with Nosto Search before, here is what you need to know about how Search Templates Starter fits into your stack.

#### Templates are hosted by Nosto

You do not need to host the built bundle yourself. When you build and push your templates using `nosto-cli`, the artifacts are uploaded to Nosto's infrastructure and served to your store visitors automatically — no CDN setup or server configuration required. Self-hosting is supported if you need it, but for most use cases Nosto's hosting is all you need.

#### The bundle is injected via the Nosto script

Your store already includes the Nosto script tag. That script is responsible for loading and injecting the search templates into your page. As long as:

1. Search Templates are enabled for your account
2. You have built and pushed your templates with `nosto-cli`
3. Your `src/config.ts` includes the correct CSS selectors to target your page's DOM elements

…the templates will appear on your site out of the box. There is nothing extra to wire up on the frontend.

#### Preview changes without going live

The development workflow separates preview from production deployment. During development, `nosto-cli` provides a watch mode (`nosto st dev`) that automatically builds and pushes your artifacts to Nosto as you save files — only the built artifacts are uploaded, not your entire repository. Your changes go to **preview mode** first and are not visible to real visitors yet. To see them yourself:

1. Navigate to your store and append `?nostodebug=true` to the URL
2. Log in to the Nosto debug toolbar that appears
3. Toggle **Preview** mode on
4. Your latest changes will now be visible as you browse the store

When you are happy with the result, you can deploy to production either from the Nosto Admin UI or directly using the CLI. This is the same workflow as the legacy Search Templates — the difference is only in how you develop and push your code locally.

### Next steps

To start developing with Search Templates Starter, you'll need **Node.js 24+** and familiarity with React/Preact. Here is the typical workflow end to end:

1. **[Getting started](getting-started.md)** — Clone the repository, install dependencies, and configure your merchant ID and CSS selectors
2. **Authenticate** — Run `nosto login` to connect the CLI to your Nosto account
3. **Develop** — Use `npm run dev` for fast component iteration in isolation, or `nosto st dev` to push changes and test them on your real store
4. **Preview** — Enable the Nosto debug toolbar on your store (`?nostodebug=true`) and toggle Preview mode to review your changes before they go live
5. **[Deploy](../deployment-and-testing/deploying.md)** — When ready, deploy from the CLI or from the Nosto Admin UI

### Best practices

#### Use separate branches for production and development

Maintain at least two long-lived branches in your repository — for example `main` for the current production state and `dev` for ongoing development. When you need to apply an urgent fix to the live store, you can branch off `main`, apply and deploy the patch, and merge back — without disrupting work in progress on `dev`. This is the same branching model you would use in any professional frontend project, and it maps naturally onto Nosto's preview/production deployment model.

#### Keep repository ownership with the active developer

The repository should be owned by whoever is actively developing the template - typically the merchant or their agency. If Nosto built the template initially, ownership should be transferred over when development is handed off. See [Repository Ownership](repository-ownership.md) for details and how to transfer it.