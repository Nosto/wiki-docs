---
description: >-
  The Nosto CLI is a command-line tool that streamlines the development workflow
  for modern and legacy Nosto Search Templates.
---

# Nosto CLI

### Why use the Nosto CLI?

If you prefer to develop the search templates on your own machine, using your own tooling such as git or eslint, you may prefer to utilize the [Nosto CLI](https://github.com/Nosto/nosto-cli) tool, [now available on NPM](https://www.npmjs.com/package/@nosto/nosto-cli).

The primary purpose of the tool is to fetch and upload the sources and build artifacts of your search templates for local development, simplifying the process and minimizing friction. It supports local builds that do not rely on the VSCode Web extension, and it includes convenient development mode which automates build-and-upload loop you would have to perform manually through VSCode Web otherwise.

Since October 2025, the Nosto CLI is the recommended way to work with Search Templates.

> **Safety Notice**\
> If you have concerns about running Nosto CLI on your machine, you can examine the [open source code on GitHub](https://github.com/Nosto/nosto-cli) to ensure it meets your security requirements.

#### Legacy Search Templates

Historically, Search Templates have been developed using a VSCode Web Extension as part of Nosto Admin UI. This introduced a number of limitations - such as inability to integrate TypeScript, ESLint or LLM tools - and Nosto is moving away from the web extension in favor of Nosto CLI and local development workflows.

Search Templates built with the web extension are now referred to as Legacy Search Templates. Their counterparts are Modern Search Templates, built with [Search Templates Starter](https://github.com/Nosto/search-templates-starter/).

Nosto CLI fully supports both modes for local development.

### Installation and Setup

Nosto CLI aims to be as user-friendly as command line tools get. You should be able to get up and running by utilizing the built-in `nosto help` and `nosto setup` commands, but a quick-start guide is also provided here.

#### Installation Options

If your template is based on the Search Templates Starter project, the Nosto CLI is already included as a dependency. In this case, you can run it directly using `npx`:

```bash
npx nosto --help
```

> **Note:** All the command examples in this article will omit `npx`, but you will need to add it every time unless you opt for a global install.

For legacy templates or if you prefer to avoid `npx`, you may install it globally:

```sh
npm install -g @nosto/nosto-cli
```

#### Authentication

The Nosto CLI supports two authentication methods - your Nosto user account or an API key.

**User Account Authentication**

```bash
nosto login
```

This opens a browser window for secure authentication. Requires 2FA enabled on your Nosto account and stores credentials in your system's home folder for 8 hours. This method works across all merchant accounts you have access to.

**API Key Authentication**

Alternatively, you can use a private Search API key in your project configuration. Public API keys are not supported as the CLI requires read-write access. You can provide your API key as part of the configuration described below. The API keys are scoped to a single merchant, but they never expire.

#### Project Configuration

For each merchant account you're working with, create a new folder and set it as the current working directory.

Create a `.nosto.json` configuration file in your project root:

```bash
nosto setup -m YOUR_MERCHANT_ID
```

This creates a minimal configuration file. You can also create it manually:

```json
{
  "merchant": "your-merchant-id"
}
```

**Required Configuration:**

* `merchant` - Your Nosto merchant ID (e.g., `shopify-12345678`)

**Optional Configuration:**

* `apiUrl` - API endpoint, defaults to production
  * Production URL: `https://api.nosto.com`
  * Staging URL: `https://api.staging.nosto.com`
  * Nosto internal development URL: `https://my.dev.nos.to/api`
* `apiKey` - Private API key for authentication (if not using user login)

> **Note:** Refer to `nosto setup` for a full list of configuration options available in your version.

**Environment Variables:** You can also use environment variables. If provided, they take precedence over the config file:

* `NOSTO_MERCHANT`
* `NOSTO_API_URL`
* `NOSTO_API_KEY`

### Development Workflow

Once configured, your development workflow typically looks like:

```bash
# Ensure you're logged in
nosto login

# Start development mode with auto-upload
nosto st dev

# Open your store and enable Nosto Debug Toolbar preview mode
# Your changes will appear automatically as you save files and reload the page
```

#### Preview Mode Setup

To see your changes on your live store:

1. **Enable Debug Toolbar**: Add `?nostodebug=true` to your store URL
2. **Enable Preview**: Toggle the "Preview" button in the debug toolbar
3. **See Changes**: Refresh your page to see updates as you save files locally

#### Development Commands

**Status Check**

```bash
nosto status
```

Shows the current status of your templates and configuration.

**Pull Remote State**

```bash
nosto st pull
```

Fetches the remote state from the Nosto cloud storage locally. Primarily intended for merchants that do not rely on git for version control.

**Development Mode**

```bash
nosto st dev
```

Watches files for changes and automatically uploads builds for preview. Essential for active development. Note that this does **not** **upload the source code** to speed up the development, only the minimal required set of files to see your changes live. After finishing your development for the day, you may want to run `nosto st push` as well.

**Push Sources (Upload)**

```bash
nosto st push
```

Builds and uploads the current state of the project to Nosto cloud storage. This includes both built artifacts and source files.

> **Note:** Refer to `nosto --help` and `nosto st --help` for more information about the commands available in your version.

### Production Deployments

At the moment, Nosto CLI does not support production deployments. You may still use the Admin UI to create deployments as usual.

### Nosto CLI and Git

The CLI tool is intended to be used in combination with Git. We recommend you create a git repository per merchant you develop for, and use that as the source of truth for your development. Then, you can use Nosto CLI to build and upload the sources and build artifacts to Nosto's cloud storage, making them immediately available on your store.

In addition, Nosto CLI automatically takes the contents of your `.gitignore` file into account when deciding which files should be uploaded to the cloud, respecting the patterns you expect it to ignore.

### Troubleshooting

#### Common Issues

**Authentication Expired:**

```bash
nosto login
```

Re-authenticate if you see permission errors.

**Wrong Merchant ID:** Check your `.nosto.json` file or `NOSTO_MERCHANT` environment variable.

**Upload Failures:**

* Ensure you have internet connectivity
* Verify your API credentials are valid
* Check that the merchant ID is correct

**Preview Not Showing:**

* Confirm debug toolbar is enabled (`?nostodebug=true`)
* Ensure preview mode is toggled on
* Try refreshing the page

#### Getting Help

* **CLI Help**: Run `nosto --help` for command information
* **GitHub Issues**: Report bugs at [github.com/Nosto/nosto-cli](https://github.com/Nosto/nosto-cli)
* **Nosto Support**: Contact support through your Nosto Admin UI

### Links and Resources

* **GitHub Repository**: [https://github.com/Nosto/nosto-cli](https://github.com/Nosto/nosto-cli)
* **NPM Package**: [https://www.npmjs.com/package/@nosto/nosto-cli](https://www.npmjs.com/package/@nosto/nosto-cli)
* **Nosto Documentation**: [https://docs.nosto.com/](https://docs.nosto.com/)
