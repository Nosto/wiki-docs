# E-commerce Events

Nosto's UGC Tracks different Commerce events to ensure we can accurately track the impact of your widgets on shopping behavior.

We recommend you start by reading our [Getting Started](https://developer.stackla.com/analytics/google-analytics-v4/getting-started/) guide.

Please take particular notice of the Commerce section of the Getting[ ](https://developer.stackla.com/analytics/v4/getting-started)Started guide, which will help you set up the necessary scripts and tagging for your website.

Learn more about which UGC widgets support the Google Analytics 4 Plugin [here](http://help.nosto.com/en/articles/7890296-google-analytics-4-authentication-configuration#h_4764c82fc5)

## E-commerce Tracking

When interacting with tiles in Nosto's UGC, we utilize the browser's local storage to store vital information about what particular products, tiles, or widgets the user showed interest in.

**We store the following information:**

* Details about the tile
* Whether the interaction with a tile is a potential conversion or not
* A 24-hour expiry associated with the potential conversion

When a user visits a product they are interested in and clicks Add to Cart, the tile that leads to that interaction is now marked as a potential conversion.

If the user has interacted with a UGC tile attributed to the product the user is looking at, our Google Analytics 4 User Properties are attached to the user's add-to-cart event.

If the product is removed from the cart, that product is no longer marked as a potential conversion event and is not included in future user property attachments.

Once the user has completed their transaction, the moment they reach the order confirmation, the tiles they showed interest in, that were marked as potential conversions, are now sent to Google Analytics 4 for storing.

Learn more about **Nosto's UGC attribution model for E-commerce** [here](https://help.nosto.com/en/articles/7890307-ugc-commerce-dashboard#h_977cdfb3c8).

### Google Analytics 4 User Properties

* nosto\_ugc\_widget
  * The ID of the widget the user interacted with
* nosto\_ugc\_media
  * The media of the tile the user interacted with
* nosto\_ugc\_tile\_id
  * The ID of the tile the user interacted with
* nosto\_ugc\_tags
  * The tags of the tile the user interacted with
* is\_nosto\_ugc\_user
  * A boolean value to determine if the user is a UGC user
* is\_nosto\_ugc\_new\_user
  * A boolean value to determine if the user is a new UGC user
* nosto\_ugc\_terms
  * The terms of the tile the user interacted with

These properties will be automatically created in the Google Property assigned upon the Google Analytics 4 plugin configuration.

### E-commerce Events

The following **E-commerce events** are tracked by the Nosto's UGC Google Analytics 4 plugin:

* `Product View` (nosto\_ugc\_product\_view)- This event is attributed to UGC when a web user is on a product page after [interacting](https://help.nosto.com/en/articles/7890307-ugc-commerce-dashboard#h_977cdfb3c8) with a UGC Widget.
* `Add to Cart`(add\_to\_cart) - This event is attributed to UGC when a web user clicks add to cart after [interacting](https://help.nosto.com/en/articles/7890307-ugc-commerce-dashboard#h_977cdfb3c8) with a UGC Widget.
* `Transaction` (purchase) - This event is attributed to Visual UGC when a web user finalizes a transaction after [interacting](https://help.nosto.com/en/articles/7890307-ugc-commerce-dashboard#h_977cdfb3c8) with a UGC Widget.

Learn more about **Nosto's UGC Commerce Dashboard** [here](https://help.nosto.com/en/articles/7890307-ugc-commerce-dashboard#h_977cdfb3c8).
