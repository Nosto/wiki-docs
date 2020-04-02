# Defining Nosto placements

You can define placements for Nosto to use via a code block called div-elements. Each element marks a location in your site where Nosto can hook into and expose onsite content.

Here is an example of a `<div>` tag on the site:

```markup
<div class="nosto_element" id="frontpage-nosto-1"></div>
```

The class needs to always reference `nosto_element` so Nosto understands that this element is available for onsite content placement. However the id `frontpage-nosto-1` is flexible but requires that each unique element-id has a matching placement defined in Nosto's admin dashboard in order to expose campaigns.

Here is an example of a page with multiple `<div>` elements:

```markup
// Three separate <divs> after another on a page

<div class="nosto_element" id="frontpage-nosto-1"></div>
<div class="nosto_element" id="frontpage-nosto-2"></div>
<div class="nosto_element" id="frontpage-nosto-3"></div>

// You can also add the class and id to an element you are already using for other purposes

<div class="sidebar nosto_element" id="nosto-sidebar">

   <h1>Hello World!</h1>
   <h2>This is the sidebar</h2>

   // You can also nest nosto elements within other wrapper elements

   <div class="nosto_element" id="nosto-sidebar-nested-1"></div>

</div>
```

