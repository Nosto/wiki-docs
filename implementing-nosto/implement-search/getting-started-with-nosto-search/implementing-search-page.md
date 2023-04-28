# Implementing Search page

## Setting Up

`index.js` should be used to bind Search on your website.

The `inputCssSelector` param can be used for this purpose. If there is an existing Search on your website, you can copy the CSS selector for it by highlighting the search box through the developer tools on your browser, and then right-click and ‘Copy selector’.

We will need to unbind any existing Search within the page first. To achieve this, a new function can be introduced (similar to the one below) that will help unbind the original Search functionality and replace it with Search. This can be done in `helpers.js`, for example.

```javascript
export function unbindOriginalSearch(selector) {
    const elements = document.querySelectorAll(selector);
    elements.forEach(el => {
        var newElement = el.cloneNode(true);
        el.parentNode.replaceChild(newElement, el);
    })
}
```

This can then be called from within `index.js`, just before the `init()` function. The call can pass in the CSS selector that we copied earlier from the website, `unbindOriginalSearch('#search_form')`

## Sort options and other configurations

All sorting options can be controlled via `config.js`, where the field on which the sorting is applied can be changed.

This is also where the default page size can be changed.

<figure><img src="../../../.gitbook/assets/Screenshot 2022-09-21 at 15.17.08.png" alt=""><figcaption><p>config.js</p></figcaption></figure>

## Components

Listed below are how the different components under `serp` and `autocomplete` will be used to render different sections on the Search Page and the Autocomplete box.

### Search Page (_`default-design/src/serp`)_

#### **BottomToolbar.jsx**

Design of the toolbar at the button of the page. This can be used for displaying the number of pages, page count, items per page, etc.

#### **Facet.jsx**

Design of the facets. For example, the product colour, size, and other filters the user may wish to apply to the search results.

#### **Loader.jsx**

Design of the loading page, while search results are retrieved and rendered. This is shown while the search results load. By default, this will be a throbber/loading icon.

#### **NoResults.jsx**

Page to show that there were no results found.

#### **Pagination.jsx**

Design for changing pages, usually shown below the _BottomToolbar_. It can be used to design the way the user changes the page.

#### **Products.jsx**

Design of how the product listings show up on the search page. This can be used to control how many products are shown on the result page, as well as the design of how they are shown.

### Search Autocomplete (_`default-design/src/autocomplete`_)

Design of the autocomplete box as the user enters their search query. This is usually shown just below the search box, and updates in real-time. It features product suggestions to help users view the top matches with their query, even before the search page loads.



### Styling

All these components can be customised further with the _.css_ files under `styles`.

#### Using autocomplete up-down arrow keys

To enable the use of up-down keys with Autocomplete, a special class defined for specifying the colour of the "current" or hovered element must be defined. `.ns-product-hovered` should be set to a colour to enable this behaviour.
