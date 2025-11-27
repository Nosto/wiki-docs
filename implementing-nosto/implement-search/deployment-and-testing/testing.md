# Testing

### How to use debug toolbar to preview search

With the [Nosto debug toolbar](https://help.nosto.com/en/articles/1441625-how-to-use-the-nosto-debug-toolbar), you can see all the changes made to your website right away. To enable this feature, simply turn on the preview mode. After saving any changes in the code editor, you will be able to see them directly on your website.

**How to use preview:**

1. Navigate to your website
2. In the URL, append `?nostodebug=true` to enable the debug toolbar
3. The Nosto debug toolbar should open up, where you will be asked to log in
4. Once you have logged in, enable the Preview toggle button at the bottom
5. You should now be able to view your changes live, via the Search box

#### Manual testing

Before each deployment search should be manually tested to ensure that everything works correctly.

**What to test?**

* Autocomplete returns results
* Search displays results, facets with counts
* You can select multiple facets (on the same field and different fields)
* Sorting is working
* Pagination is working

Test both mobile & desktop view using Chrome [device simulation](https://developer.chrome.com/docs/devtools/device-mode/).
