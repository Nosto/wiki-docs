# Terminology

## Action

_Action_ in the Session API context means fetching recommendations for a specific view. For example when a visitor navigates from the front page to a product view you would most likely fetch recommendations related to a [product](spa-basics-tracking-events.md#upon-viewing-a-product). This would be considered as an _Action_. Setting the [cart contents](spa-basics-managing-sessions.md#setting-the-cart) however would not be considered as action.

It's also good to keep in mind that each _Action_ is considered as a page view in Nosto's analytics. It's recommended to perform an action only once per route change to keep the analytics consistent and comparable to traditional "static" e-commerce stores.  

## Route change

_Route change_ in the context of this documentation means, as it stands, when the navigational route \(url\) of the application is changed. In traditional web sites this would be a \(full\) page load.

