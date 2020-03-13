Nosto utilizes tracks what a customer is searching for by reading the search query from the URL's query parameter or from the page source.

* When the search term exists as a part of the URL's query parameters e.g. `https://www.example.com?q=searchterm`, Nosto can be configured to read the search term and you can skip the search term tagging.
* When the search term exists as a part of the URL e.g. `https://www.example.com/searchterm`, Nosto is unable to read it from the URL and you will need to tag as a part of the page source.

```html
<div class="nosto_page_type" style="display:none">search</div>
<div class="nosto_search_term" style="display:none">green shoes</div>
```