# Using the Nosto CLI

The [Nosto CLI](https://github.com/Nosto/nosto-cli) is a powerful command-line tool that streamlines the development and deployment workflow for Search Templates Starter projects. It provides seamless integration between your local development environment and your Nosto account.

## Why use the Nosto CLI?

The Nosto CLI is the recommended way to deploy Search Templates Starter projects because it:

-   **Automates deployment workflows** - Upload builds and preview changes without manual steps
-   **Enables live preview** - See your changes on your actual store with the debug toolbar
-   **Integrates with Git** - Respects your `.gitignore` and version control workflow
-   **Supports team collaboration** - Multiple developers can work on the same templates
-   **Provides safety features** - Preview mode ensures changes are tested before going live

Since October 2025, the Nosto CLI is the recommended way to work with Search Templates.

> **Safety Notice**  
> If you have concerns about running Nosto CLI on your machine, you can examine the [open source code on GitHub](https://github.com/Nosto/nosto-cli) to ensure it meets your security requirements.

## Installation and Setup

### Installation Options

The Nosto CLI is already included as a dependency in the Search Templates Starter project, so you can run it using `npx`:

```sh
npx nosto --help
```

For frequent use, you may prefer to install it globally:

```sh
npm install -g @nosto/nosto-cli
```

### Authentication

The Nosto CLI supports two authentication methods:

#### User Account Authentication (Recommended)
```bash
npx nosto login
```
This opens a browser window for secure authentication. Requires 2FA enabled on your Nosto account and stores credentials for 8 hours. This method works across all merchant accounts you have access to.

#### API Key Authentication
Alternatively, you can use a private Search API key in your project configuration. Public API keys are not supported as the CLI requires read-write access.

### Project Configuration

Create a `.nosto.json` configuration file in your project root:

```bash
npx nosto setup -m YOUR_MERCHANT_ID
```

This creates a minimal configuration file. You can also create it manually:

```json
{
  "merchant": "your-merchant-id",
  "apiUrl": "https://api.nosto.com"
}
```

#### Configuration Options

**Required Configuration:**
- `merchant` - Your Nosto merchant ID (e.g., `shopify-12345678`)

**Optional Configuration:**
- `apiUrl` - API endpoint (defaults to production `https://api.nosto.com`)
- `apiKey` - Private API key for authentication (if not using user login)

**Environment Variables:**
You can also use environment variables (they take precedence over the config file):
- `NOSTO_MERCHANT`
- `NOSTO_API_URL` 
- `NOSTO_API_KEY`

## Development Workflow

### Typical Development Flow

Once configured, your development workflow typically looks like:

```bash
# Navigate to your project
cd search-templates-starter

# Ensure you're logged in
npx nosto login

# Start development mode with auto-upload
npx nosto st dev

# Open your store and enable Nosto Debug Toolbar preview mode
# Your changes will appear automatically as you save files
```

### Preview Mode Setup

To see your changes on your live store:

1. **Enable Debug Toolbar**: Add `?nostodebug=true` to your store URL
2. **Login**: Authenticate when prompted in the debug toolbar
3. **Enable Preview**: Toggle the "Preview" button in the debug toolbar
4. **See Changes**: Refresh your page to see updates as you save files locally

### Development Commands

#### Development Mode
```bash
npx nosto st dev
```
Watches files for changes and automatically uploads builds for preview. Essential for active development.

#### Manual Upload
```bash
npx nosto st upload
```
Uploads the current build without watching for changes. Useful for one-time deployments or testing.

#### Status Check
```bash
npx nosto status
```
Shows the current status of your templates and configuration.

## Production Deployment

### Promoting to Production

The Nosto CLI only handles preview deployments. To promote your changes to production:

1. **Test thoroughly** in preview mode
2. **Navigate** to Nosto Admin UI > Search > Templates  
3. **Click** "Deploy latest and launch live"

> **Important:** Always test your changes thoroughly in preview mode before promoting to production, as this affects all your store visitors.

### Deployment Safety

- **Preview First**: All CLI uploads go to preview mode initially
- **Admin Control**: Production deployments require manual approval in the Admin UI
- **Rollback Available**: Previous versions can be restored from the Admin UI if needed

## Git Integration

### Recommended Workflow

The Nosto CLI is designed to work seamlessly with Git:

```bash
# Create feature branch
git checkout -b feature/new-search-layout

# Make changes and test with CLI
npx nosto st dev

# Commit and push changes
git add .
git commit -m "Add new search layout"
git push origin feature/new-search-layout

# Deploy from main branch after review
git checkout main
git merge feature/new-search-layout
npx nosto st upload
```

### File Handling

The CLI automatically respects your `.gitignore` file when uploading, ensuring that:
- Node modules and build artifacts aren't uploaded unnecessarily
- Sensitive files remain local
- Only source code and assets are deployed

## Troubleshooting

### Common Issues

**Authentication Expired:**
```bash
npx nosto login
```
Re-authenticate if you see permission errors.

**Wrong Merchant ID:**
Check your `.nosto.json` file or `NOSTO_MERCHANT` environment variable.

**Upload Failures:**
- Ensure you have internet connectivity
- Verify your API credentials are valid
- Check that the merchant ID is correct

**Preview Not Showing:**
- Confirm debug toolbar is enabled (`?nostodebug=true`)
- Ensure preview mode is toggled on
- Try refreshing the page

### Getting Help

- **CLI Help**: Run `npx nosto --help` for command information
- **GitHub Issues**: Report bugs at [github.com/Nosto/nosto-cli](https://github.com/Nosto/nosto-cli)
- **Nosto Support**: Contact support through your Nosto Admin UI

## Links and Resources

- **GitHub Repository**: [https://github.com/Nosto/nosto-cli](https://github.com/Nosto/nosto-cli)
- **NPM Package**: [https://www.npmjs.com/package/@nosto/nosto-cli](https://www.npmjs.com/package/@nosto/nosto-cli)
- **Nosto Documentation**: [https://docs.nosto.com/](https://docs.nosto.com/)
