**Introduction**

Implementing Nosto on a single-page application may seem like a daunting task but is actually trivial. The content below hinges on a clear understanding of a few key VueJS concepts:

* [Instance Lifecycle Hooks](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks)
* [Inline Templates](https://vuejs.org/v2/guide/components-edge-cases.html#Inline-Templates)
* [Global Mixins](https://vuejs.org/v2/guide/mixins.html#Global-Mixin)
* [Server Side Rendering](https://vuejs.org/v2/guide/ssr.html)

We recommend looking through [our companion app on Codepen](https://codepen.io/mridang/project/editor/ArMkgm).

<img width="1440" alt="screen shot 2018-10-22 at 14 35 43" src="https://user-images.githubusercontent.com/327432/47290372-d0bd9280-d607-11e8-9e0f-4f7447bcd4c5.png">


## Tagging Structure

The example below shows a simple tagging structure for all pages. Each page should have the the customer, cart, page-type and - if using multi-currency, the variant-id tagging.

On product and category pages, you would also need to ensure that the appropriate tagging is in place. An example of that was omitted due to the scope of the article.

```html
<body>
  <div id="app">
    ...
    ...
    ...
    <nosto page-type="front" inline-template>
      <div>
        <div class="nosto_page_type" style="display:none">{{ page_type }}</div>
        <div class="nosto_customer" style="display:none">
	  <div class="first_name">{{ customer.first_name }}</div>
  	  <div class="last_name">{{ customer.last_name }}</div>
	  <div class="email">{{ customer.email }}</div>
	  <div class="customer_reference">{{ customer.reference }}</div>
	  <div class="marketing_permission">{{ customer.newsletter }}</div>
	</div>
        <div class="nosto_cart" style="display:none">
          <div class="line_item" v-for="item in cart">
            <span class="product_id">{{ item.product_id }}</span>
            <span class="quantity">{{ item.quantity }}</span>
            <span class="name">{{ item.name }}</span>
            <span class="unit_price">{{ item.price }}}</span>
            <span class="price_currency_code">{{ item.currency }}</span>
          </div>
        </div>
        <nosto-slot nid="frontpage-nosto-1" inline-template>
          <div class="nosto_element no-inject" id="frontpage-nosto-1">
	    ...
            ...
          </div>
        </nosto-slot>
        <nosto-slot nid="frontpage-nosto-2" inline-template>
          <div class="nosto_element no-inject" id="frontpage-nosto-1">
	    ...
            ...
          </div>
        </nosto-slot>
      </div>
    </nosto>
  </div>
  <script src="/scripts/nosto.js" type="module"></script>
  <script src="/scripts/components/nosto.js" type="module"></script>
  <script src="/scripts/components/slot.js" type="module"></script>
  <script type="module">
    import NostoWrapper from './scripts/components/nosto.js'
    import NostoSlot from './scripts/components/slot.js'
    import Nosto from './scripts/nosto.js'

    Nosto.setResponseMode('JSON_750x750');
    Vue.component('nosto', NostoWrapper);
    Vue.component('nosto-slot', NostoSlot);
    // Vue mixin that is available globally to all components allowing
    // them to reload Nosto. This method should be used to reload
    // Nosto when there is s change in state. A change in state could
    // be adding a product to the cart or dynamically navigating to
    // a category
    Vue.mixin({
      methods: {
        reloadNosto: function() {
          Nosto.loadRecommendations();
        },
        showPopup: function() {
          Nosto.showPopup();
        }
      }
    });

    new Vue({el: '#app'});
  </script>
```



**Note**: Nosto indexes your product pages by crawling your site. You must use server-side rendering to tag your product pages. Nosto's crawler doesn't execute Javascript so the markup must be generated server-side.



## Implementing Recommendations

Implementing recommendations on a single-page application requires careful orchestration as compared to the other features.

On a regular site, each Nosto recommendation is identified by an empty DIV placeholder whereinto all the recommendations are injected. The Nosto script - upon DOM loaded - finds all elements having the class `nosto_element` ,fetches the content for those elements and then injects the raw markup into those placeholders. Both the styling and the configuration of the recommendations are configured via the Nosto administration interface

In the context of a single-page application built atop a framework like VueJS, only the configuration of the recommendations should be maintained via the Nosto administration interface, while the actual styling could be maintained as a part of your application. This approached required structuring your project in a certain way to allow loading recommendations seamlessly.

The project rough structure of a page with recommendations resembles this:

```html
<body>
  <div id="app">
    ...
    ...
    ...
    <nosto page-type="front" inline-template>
      <div>
        <div class="nosto_page_type">{{ page_type }}</div>
        <nosto-slot nid="frontpage-nosto-1" inline-template>
          <div class="nosto_element no-inject" id="frontpage-nosto-1">
	    ...
            ...
          </div>
        </nosto-slot>
        <nosto-slot nid="frontpage-nosto-2" inline-template>
          <div class="nosto_element no-inject" id="frontpage-nosto-1">
	    ...
            ...
          </div>
        </nosto-slot>
      </div>
    </nosto>
  </div>
```



**Note**: Each element must have the `no-inject` class along with the `nosto_element` class. This additional class, instructs the script to not inject any markup into those placeholders. As you will be require JSON, you must remember to set the correct response mode. This can be achieved by 

Every view would contain a single wrapper element which in this case is identified by the component named `nosto`. All recommendation placeholders as identified by component named `nosto-slot` should exist on within the wrapper `nosto` component.

**Deep Dive**: When each `nosto-slot` component is attached the DOM, it registers it's identifier to a global singleton class. This global singleton class contains a map of slot identifiers and callbacks. When the wrapper `nosto` component had been loaded (along with all child elements), it invoked the JS API to load recommendations for the specified placeholders. Both the wrapper component and the slot component are simple components that don't have their own template as instructed by the `inline-template` directive. When you navigate to a new route, each slot component, before being removed from the DOM, unregisters itself from the global single class. We recommend reading more about Vue's [Instance Lifecycle Hooks](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks).

**Deep Dive**: As there are scenarios where you may manually need to re-load the recommendations, a helper method called `reloadNosto` is exposed via a global mixin. All components on the DOM will have access this helper method and can force reload the recommendations.



## Implementing Popups

Implementing popups are rather simple as compared to implementing recommendations. Popups are simple overlays served by Nosto.

As the content and the delivery of the content are managed by Nosto, all that needs to be ensured is that the correct events are dispatched.

**Deep Dive**: As popups can be manually triggered from any component, the method to trigger a popup is exposed by a global mixin containing a method called `showPopup`. All components on the DOM will have access to this helper method and can force show a popup by invoking this method.