# Tracking Nosto with Google Analytics Enhanced Ecommerce

## Outline

This document describes how to implement basic Enhanced Ecommerce tracking \(product list impressions, product clicks and add to cart events\) for Nosto recommendations using Google Analytics. This guide has been written with the assumption that your store uses [Google Tag Manger \(GTM\)](http://tagmanager.google.com/) and `dataLayer`.

The basic idea of the implementation is that product lists are created in Nosto’s templates, then moved to the global scope \(\_targetWindow\) and finally pushed to the dataLayer via GTM and tracked with Google Analytics \(GA\).

You should have basic knowledge about javascript and GTM in order to use this guide efficiently.

## Configuration

GTM must be installed into your store. Also make sure the Enhanced Ecommerce is enabled in your GA account.

## Implementation - Nosto

### Initialize javascript variables and list name

Place the following snippet into the recommendation template just before the product loop \(`#foreach($product in $products)`\)

```markup
#set($gtmListName =  "Nosto - $!divId")
<script>
var productDataForGtm = {
    impressions: [],
    listName: "$gtmListName"
};
var nostoCurrency = '';
</script>
```

### Push product data into productDataForGtm variable

This snippet must be placed inside the product loop, for example right in the beginning of the product loop .

**Important! In order to keep you tracking and reporting consistent make sure you use the same source value for the id of the product for all tracking events**

```markup
#set($jsonProduct = "{name: '$!product.name',id: '$!product.productId',price: '$!product.price',brand: '$!product.brand',category: '', variant: '', url: '$!product.url', position: $!velocityCount}")
<script>
        productDataForGtm.impressions.push({
            name: "$!product.name",
            id: "$!product.productId",
            price: "$!product.price.asNumber",
            brand: "$!product.brand",
            position: $!velocityCount
        });
        nostoCurrency = '$!product.currencyCode';
</script>
```

### Add `onClick` event to links

Each link to a product page needs to call `nostoTrackRecoClick($jsonProduct, '$gtmListName')`. A link to a product page should look something like below.

```markup
<a onClick="nostoTrackRecoClick($jsonProduct, '$gtmListName')"  href="$!product.url" class="nosto-product-brand">$!product.brand.truncate($!props.display.truncateBrandNameAfter)</a>
```

In order to track cart additions from the recommendations you must call `nostoTrackAddToCart($jsonProduct, '$gtmListName');`.

```markup
<button onclick="Nosto.addProductToCart('$!product.productId', this); nostoTrackAddToCart($jsonProduct, '$gtmListName'); return false;" class="nosto-btn">$!props.display.buyButtonCopy</button>
```

### Expose the impression data to the `_targetWindow`

The impression data must be available in javascript's `window` object for the GTM to be able to read the data. In order to expose `productDataForGtm` to `window` you need to use `_targetWindow` object. Place the following snippet to the end of the template just before closing `#end` condition.

```markup
<script>
    (function(){var name="nostoGtmQueue";_targetWindow[name]=_targetWindow[name]||function(cb){(_targetWindow[name].q=_targetWindow[name].q||[]).push(cb);};})();
    _targetWindow.nostoGtmQueue(productDataForGtm);

    if (typeof _targetWindow['nostoCurrency'] == "undefined" || !_targetWindow['nostoCurrency']) {
        _targetWindow['nostoCurrency'] = nostoCurrency;
    }
</script>
```

## Google Tag Manager

You will need one custom tag and three trigger tags in GTM for the tracking to be complete.

### Nosto - tracking tag

Add following javascript snippet as a custom html tag and configure the tag to be triggered in all pages on page view.

```markup
<script>
nostoTrackAddToCart = function(product, list) {
    dataLayer.push({
        'event': 'nostoAddToCart',
        'ecommerce': {
            'currencyCode': 'EUR',
            'add': {                                // 'add' actionFieldObject measures.
                'products': [{                        //  adding a product to a shopping cart.
                    'name': product.name,
                    'id': product.id,
                    'price': product.price,
                    'brand': product.brand,
                    'quantity': 1
                }]
            }
        }
    });
}
nostoTrackRecoClick = function(product, list) {
    dataLayer.push({
        'event': 'nostoProductClick',
        'ecommerce': {
            'click': {
                'actionField': {'list': list},      // Optional list property.
                'products': [{
                    'name': product.name,                      // Name or ID is required.
                    'id': product.id,
                    'price': product.price,
                    'brand': product.brand,
                    'position': product.position
                }]
            }
        },
        'eventCallback': function() {
            location = product.url;
        }
    });
}

nostojs(function(api){
    api.listen("postrender", function(postRenderElements){
        if (typeof nostoGtmQueue != "undefined" && nostoGtmQueue != null && typeof nostoGtmQueue == "function" && typeof nostoGtmQueue.q == "object" ) {

            var impressions = [];
            for (i = 0; i < nostoGtmQueue.q.length; i++) {
                var slotQueue = nostoGtmQueue.q[i];
                  if (slotQueue.impressions != null && typeof slotQueue.impressions == "object")                     {
                    for (x = 0; x < slotQueue.impressions.length; x++) {
                          var product = slotQueue.impressions[x];
                              product.list = slotQueue.listName;
                        impressions.push(product);
                    }
                }
            }
            var currency = 'EUR';
            if (typeof nostoCurrency != "undefined" && nostoCurrency != null) {
                currency = nostoCurrency;
            }

            var nostoEcomData = {
                'ecommerce' : {
                    'currencyCode' : currency,
                    'impressions': impressions
                },
                'event': 'nostoEcomImpression'
            }
            dataLayer.push(nostoEcomData);
        }
    });
});
</script>
```

![gtm\_nosto\_tracking\_tag](https://user-images.githubusercontent.com/15191701/39805607-ff40eeaa-537f-11e8-8fa0-e6bdf733048d.png)

### Nosto - trigger product list view

Create new tag with following configuration

* Tag type: Universal Analytics
* Category: Ecommerce \(or whatever you prefer\)
* Action: Product list impression \(or whatever you prefer\)
* Non-interaction hit: True \(product list impression will not be treated as interaction\)
* Google Analytics Settings: your GA account \(it's highly recommended to configure GA account as a variable\)
* Enable overriding settings in this tag: checked
* More settings &gt; Ecommerce &gt; Enable Enhanced Ecommerce Features: True
* More settings &gt; Ecommerce &gt; Use Data Layer: checked

![google\_tag\_manager](https://user-images.githubusercontent.com/15191701/42629726-13c31a6c-85dd-11e8-8417-beff9f04f1fc.png)

Triggering

* Trigger type: Custom event
* Event name: nostoEcomImpression
* This trigger fires on: All Custom Events

![gtm\_nosto\_trigger\_for\_impressions](https://user-images.githubusercontent.com/15191701/39805612-036c66d0-5380-11e8-9f66-742c31eb3941.png)

### Nosto - trigger product click

Create new tag with following configuration

* Tag type: Universal Analytics
* Category: Ecommerce \(or whatever you prefer\)
* Action: Product list click \(or whatever you prefer\)
* Google Analytics Settings: your GA account
* Enable overriding settings in this tag: checked
* More settings &gt; Ecommerce &gt; Enable Enhanced Ecommerce Features: True
* More settings &gt; Ecommerce &gt; Use Data Layer: checked

Triggering

* Trigger type: Custom event
* Event name: nostoProductClick
* This trigger fires on: All Custom Events

### Nosto - trigger cart addition

Create new tag with following configuration

* Tag type: Universal Analytics
* Category: Ecommerce \(or whatever you prefer\)
* Action: Product list add to cart \(or whatever you prefer\)
* Google Analytics Settings: your GA account
* Enable overriding settings in this tag: checked
* More settings &gt; Ecommerce &gt; Enable Enhanced Ecommerce Features: True
* More settings &gt; Ecommerce &gt; Use Data Layer: checked

Triggering

* Trigger type: Custom event
* Event name: nostoAddToCart
* This trigger fires on: All Custom Events

