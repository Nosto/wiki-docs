# Testing & deployment

## **Testing**

### How to use debug toolbar to preview search

With the [Nosto debug toolbar](https://help.nosto.com/en/articles/1441625-how-to-use-the-nosto-debug-toolbar), you can see all the changes made to your website right away. To enable this feature, simply turn on the preview mode. After saving any changes in the code editor, you will be able to see them directly on your website.

#### How to use preview:

1. Navigate to your website
2. In the URL, append `?nostodebug=true` to enable the debug toolbar
3. The Nosto debug toolbar should open up, where you will be asked to log in
4. Once you have logged in, enable the Preview toggle button at the bottom
5. You should now be able to view your changes live, via the Search box

### Manual testing <a href="#what-to-test" id="what-to-test"></a>

Before each deployment search should be manually tested to ensure that everything works correctly.

#### What to test?

* Autocomplete returns results
* Search displays results, facets with counts
* You can select multiple facets (on the same field and different fields)
* Sorting is working
* Pagination is working

Test both mobile & desktop view using Chrome [device simulation](https://developer.chrome.com/docs/devtools/device-mode/).

## **Deployment**

When you finished working on search implementation & carefully tested everything using [Nosto debug toolbar](https://help.nosto.com/en/articles/1441625-how-to-use-the-nosto-debug-toolbar), it's time to deploy everything.

#### How to deploy?

1. Navigate to the `Nosto Admin UI` > `Search` > `Templates`
2. Click on `Deploy latest and launch live`

{% hint style="info" %}
It can take up to 15 minutes for deployment to be visible
{% endhint %}

## **Rollback previous deployment**

If the most recent update doesn't work properly, you have the option to revert to any previous update. Reverting won't alter the source, which means you can deploy the latest changes again simply by clicking on the main deployment button.

### How to revert deployment

1. Navigate to the `Nosto Admin UI` > `Search` > `Templates`
2. Click on the desired deployment, click on `...` and then on `Redeploy`

## **Disable Templates**

If your latest update doesn't work and you don't have a previous working version to go back to, or if you want to completely remove the search function, you can turn off the search templates for a temporary period.

Reverting won't alter the source, which means you can deploy the latest changes again simply by clicking on the main deployment button.

#### How to disable templates?

1. Navigate to the `Nosto Admin UI` > `Search` > `Templates`
2. Click on the `Disable Templates` button.
