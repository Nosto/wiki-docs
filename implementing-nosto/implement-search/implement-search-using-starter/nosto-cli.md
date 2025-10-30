# Using the Nosto CLI

The [Nosto CLI](https://github.com/Nosto/nosto-cli) is a powerful command-line tool that allows you to develop and deploy your search templates directly to your Nosto account. This guide covers the essential commands for running the Search Templates Starter with the Nosto CLI.

## Installation

The Nosto CLI is already included as a dependency in this project, so you can run it using `npx nosto`.

Alternatively, you can install it globally:

```sh
npm install -g @nosto/nosto-cli
```

## Deployment Workflow

Using the Nosto CLI, you can deploy the template straight to your store in a preview mode. Any changes you make will only be visible with the Nosto Debug Toolbar and preview mode enabled on your site.

### 1. Login to Nosto

First, you need to authenticate with your Nosto account.

```bash
npx nosto login
```

This command will open a browser window for you to complete the login process.

### 2. Setup the Project

Next, create a configuration file for your project. This links your local repository to a specific Nosto merchant account.

```bash
npx nosto setup -m YOUR_MERCHANT_ID
```

Replace `YOUR_MERCHANT_ID` with your actual Nosto account ID.

### 3. Run in Development Mode

To see your changes live on your site, run the `dev` command:

```bash
npx nosto st dev
```

While this command is running, any changes you save to your local files will be automatically uploaded as a preview. You can see the changes by refreshing your store page with the Nosto Debug Toolbar enabled and "Preview" mode toggled on.

### 4. Production Deployments

Once you are happy with your changes in preview mode, you can create a production deployment from the Nosto Admin UI. Navigate to the Search Templates page, and you will be able to promote your latest preview to production.
