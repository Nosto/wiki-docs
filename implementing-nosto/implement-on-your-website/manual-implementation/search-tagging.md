# Adding the Search Tagging

Nosto utilizes tracks what a customer is searching for by reading the search query from the URL's query parameter or from the page source.

* When the search term exists as a part of the URL's query parameters e.g. `https://www.example.com?q=searchterm`, Nosto can be configured to read the search term and you can skip the search term tagging.
* When the search term exists as a part of the URL e.g. `https://www.example.com/searchterm`, Nosto is unable to read it from the URL and you will need to tag as a part of the page source. When you implement the tagging for the search term, remember that it is untrusted user input added as part of your page html and it should be html-encoded to prevent XSS vulnerabilities on the site.\


```markup
<div class="nosto_page_type" style="display:none" translate="no">search</div>
<div class="nosto_search_term" style="display:none" translate="no">green shoes</div>
```
