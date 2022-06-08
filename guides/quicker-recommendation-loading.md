# Quicker recommendation loading

This documentation walks you through the steps for setting up `nostojs` client script on your theme for avoiding delays in loading recommendations

## Approach

Please follow the steps outlined below for setting up your theme:

1.  Place the below content inside the `head` section of `theme.liquid` file, above the line that says `{{ content_for_header }}`.

    ```javascript
     <script type="text/javascript">
         (() => {window.nostojs=window.nostojs||(cb => {(window.nostojs.q=window.nostojs.q||[]).push(cb);});})();
     </script>
     <script type="text/javascript">
         nostojs(api => api.setAutoLoad(false));
     </script>
    ```

    This accomplishes two results:\
    i. Initializes `nostojs` JavaScript api\
    ii. Stops Nosto from loading recommendations by default\

2.  Place the following snippet, in the theme liquid template files that contains `nosto_element` elements, below all the `nosto_element` snippets:

    ```javascript
        <script type="text/javascript">nostojs(api => api.loadRecommendations())</script>
    ```

    This line will make sure Nosto loads recommendations on after the `nosto_element` elements are loaded.
