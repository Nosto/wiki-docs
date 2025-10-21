---
description: >-
  Nosto CLI is a command line interface tool created with TypeScript and
  designed to reduce the friction of developing Nosto Search Templates.
---

# Using Nosto CLI

## **Why use the CLI tool?**

If you prefer to develop the search templates on your own machine, using your own tooling such as git or eslint, you may prefer to utilize the [Nosto CLI](https://github.com/Nosto/nosto-cli) tool, [now available on NPM](https://www.npmjs.com/package/@nosto/nosto-cli).

The primary purpose of the tool is to fetch and upload the sources and build artifacts of your search templates for local development, simplifying the process and minimizing friction. It supports local builds that do not rely on the VSCode Web extension, and it includes convenient dev mode which automates build-and-upload loop you would have to perform manually through VSCode Web otherwise.

Since October 2025, Nosto CLI is our recommended way to work with the search-templates.

> **Safety Notice**\
> If you are concerned about running custom code produced by Nosto on your machine, we welcome you to examine the source code of Nosto CLI [available on our GitHub](https://github.com/Nosto/nosto-cli) and ensure it does not produce any undesired side-effects.

### Getting Started

Nosto CLI aims to be as user-friendly as CLI tools get. You should be able to get up and running by utilizing the built-in `help` and `setup` commands, but a quick-start guide is also provided here.

To start with, you may create an empty folder for your project; or you may clone your git repository to work with.

```bash
# Install the CLI tool:
npm i @nosto/nosto-cli -g

# Login to Nosto
# You will see the browser window open with further instructions.
nosto login

# Run the tool against a project directory:
nosto status /path/to/project

# Alternatively, the current working directory is used
cd /path/to/project && nosto status
```

After you have everything setup and ready to go, your typical development flow may look something like this:

```bash
# Navigate to your project
cd /path/to/project

# Fetch the latest state of the repo
git pull

# Run the Nosto dev mode that will automatically watch files, rebuild and upload
nosto st dev

# Open your store in a browser, refresh and see the results!
```

#### Authentication

Nosto CLI supports two modes of authentication - user account login and an API key-based authentication flow. User account auth is performed via `nosto login`, it requires 2FA enabled on your account, and it stores your login details for 8 hours in your system's home folder. This is the simplest way of authenticating that works across different merchant accounts, and is the recommended way to authenticate.

Alternatively, Nosto CLI also supports authentication via a private Search API key. Note that the keys marked as public are explicitly not allowed, as the CLI requires read-write access. You can provide an API key per-merchant in a configuration file (see below), and it's valid as long as the key itself stays valid. User account authentication is not needed in this case.

#### Configuration

The recommended way to provide the configuration is a config file in the project folder, named `.nosto.json`. Alternatively, environmental variables can be used. If both are present, the environment takes precedence.

#### Required configuration

At the minimum, one option is required: Merchant ID. If you're targeting an environment other than production, an API URL will also be required.

> To quickly create a minimal configuration file, you may use the following command: `nosto setup -m merchant-id`

**Merchant ID:**

Public ID of the target merchant. For example, `shopify-0000000` .

> Property name in the config file: `merchant`\
> Property name in the env variable: `NOSTO_MERCHANT`

**API URL (Optional):**

By default, the CLI will try to contact production as the base URL. You may need to specify a different URL to target the correct environment.

* Production URL: `https://api.nosto.com`
* Staging URL: `https://api.staging.nosto.com`
* Nosto internal development URL: `https://my.dev.nos.to/api`

> Property name in the config file: `apiUrl`\
> Property name in the env variable: `NOSTO_API_URL`

**API Key (Optional):**

By default, the CLI will use your user credentials created by `nosto login`. If the API token is provided for a given project, it will be used instead.

This is your access key for the target merchant. Specifically, a private Nosto API\_APPS token that you can find in the merchant admin settings.

> Property name in the config file: `apiKey`\
> Property name in the env variable: `NOSTO_API_KEY`

### Nosto CLI and Git

The CLI tool is intended to be used in combination with Git. We recommend you create a git repository per merchant you develop for, and use that as the source of truth for your development. Then, you can use Nosto CLI to build and upload the artifacts and/or the sources to Nosto's cloud storage, making them immediately available on your store.

In addition, Nosto CLI automatically takes the contents of your `.gitignore` file into account when deciding which files should be uploaded to the cloud, respecting the patterns you expect it to ignore.

### Links

Public Git repository for Nosto CLI: [https://github.com/Nosto/nosto-cli](https://github.com/Nosto/nosto-cli)

NPM page for Nosto CLI: [https://www.npmjs.com/package/@nosto/nosto-cli](https://www.npmjs.com/package/@nosto/nosto-cli)
