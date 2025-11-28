# Deploying

When you finished working on search implementation & carefully tested everything using [Nosto debug toolbar](https://help.nosto.com/en/articles/1441625-how-to-use-the-nosto-debug-toolbar), it's time to deploy everything.

### Production Deployment

The Nosto CLI only handles preview deployments. To promote your changes to production:

1. **Test thoroughly** in preview mode
2. **Navigate** to Nosto Admin UI > Search > Templates
3. **Click** "Deploy latest and launch live"

> **Important:** Always test your changes thoroughly in preview mode before promoting to production, as this affects all your store visitors. It can take up to 15 minutes for deployment to be visible.

#### Deployment Safety

* **Preview First**: All CLI uploads go to preview mode initially
* **Admin Control**: Production deployments require manual approval in the Admin UI
* **Rollback Available**: Previous versions can be restored from the Admin UI if needed

### Rollback previous deployment

If the most recent update doesn't work properly, you have the option to revert to any previous update. Reverting won't alter the source, which means you can deploy the latest changes again simply by clicking on the main deployment button.

#### How to revert deployment

1. Navigate to the `Nosto Admin UI` > `Search` > `Templates`
2. Click on the desired deployment, click on `...` and then on `Redeploy`

### Disable Templates

If your latest update doesn't work and you don't have a previous working version to go back to, or if you want to completely remove the search function, you can turn off the search templates for a temporary period.

Reverting won't alter the source, which means you can deploy the latest changes again simply by clicking on the main deployment button.

**How to disable templates?**

1. Navigate to the `Nosto Admin UI` > `Search` > `Templates`
2. Click on the `Disable Templates` button.
