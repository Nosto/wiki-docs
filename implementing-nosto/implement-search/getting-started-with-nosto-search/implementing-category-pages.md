# Implementing Category pages

Category merchandising can also be added through the same `init()` configuration. It is best to have already-existing category pages and disable or hide current results rendering in category pages to achieve the best results.&#x20;

There are four different fields that need to be added to the `init()` function.

`categoryCssSelector` is similar to `contentCssSelector`, it directs where to render the results.

`categoryQuery` gets the category path from the document dynamically.

`isCategoryPage` is the logic that determines whether the current page is a category page and whether category results should be rendered.



```javascript
...
    contentCssSelector: '#content',
    categoryCssSelector: '#categoryContent',
    categoryQuery: () => {
        // In this example, the category path is placed in an HTML div element.
        const categryPath = document.querySelector('div.category_path')
        return {
            name: 'category',
            products: {
                categoryPath: "Accueil/Militaire - Forces de l'ordre/Gendarmerie/Matériels/",
            }
        }
    },
    isCategoryPage: () => {
        // Logic to check whether the current page is a category page.
        return location.search.includes('/category/')
    },
...
```

Also included in the default design is a `categoryComponent`, which is responsible for rendering the results of categories.
