# Setup recommendation
This documentation walks you through the steps for setting up `nostojs` client script on your theme for avoiding delays in loading recommendations

## Approach
Please follow the steps outlined below for setting up your theme:
1. Place the script content provided [here](https://docs.nosto.com/techdocs/apis/frontend/implementation-guide-session-api/spa-basics-setting-up#including-the-script) inside the `head` section of `theme.liquid` file, above the line that says `{{ content_for_header }}`. This accomplishes two results:
   a. Initializes `nostojs` JavaScript api
   b. Stops Nosto from loading recommendations by default
2. Place the following snippet, in the theme liquid template files that contains `nosto_element` elements, below all the `nosto_element`  snippets:
    ```javascript
        <script type="text/javascript">nostojs(api => api.loadRecommendations())</script>
    ```
    This line will make sure Nosto loads recommendations on after the `nosto_element` elements are loaded.