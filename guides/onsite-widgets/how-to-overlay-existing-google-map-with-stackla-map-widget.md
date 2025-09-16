# How to overlay existing Google Map with the UGC Map Widget

{% hint style="warning" %}
You are reading the Classic Widget Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../widgets-nextgen/).
{% endhint %}

* [Overview](how-to-overlay-existing-google-map-with-stackla-map-widget.md#overview)
* [Creating a Map Widget](how-to-overlay-existing-google-map-with-stackla-map-widget.md#creating-your-first-map-widget)
* [Changing the look and feel](how-to-overlay-existing-google-map-with-stackla-map-widget.md#changing-look-and-feel)
* [Overlaying the map on your existing Google Map](how-to-overlay-existing-google-map-with-stackla-map-widget.md#overlaying-the-map-on-your-existing-google-map)
* [Final Demo](how-to-overlay-existing-google-map-with-stackla-map-widget.md#final-demo)

## Overview

Map widget provides a visualisation of where content is happening.

In this tutorial, we will show you how to create a Map Widget and overlay it on top of your existing Google Map.

## Creating your first Map widget

![](../../.gitbook/assets/create-new-widget.png)

Once the Maps plugin is installed, you will see a new widget type in the widget creation page.

Simply choose Map and click **Create Widget**

And don't forget to name your widget.

### Connecting to a content widget

![](../../.gitbook/assets/change-display-type.png)

By default, Map is simply a map that shows where the content is created. However, you can connect the Map widget with one of our content widgets, e.g. a Waterfall widget. To do so, change the display type from \[Map only] to \[Map + Feed] then select a content widget to connect with.

Once the widgets are connected, the content widget gets notified when a user clicks on the pin on the Map widget.

### Previewing the connected widgets

![](../../.gitbook/assets/map-on-jsbin.png)

To view the connected widgets, save your current settings first then go to the Embed Code tab. If you chose to use an existing widget as content widget, you might have already got the widget embed code running in your page. If that's the case, simply choose \[Map Only] and copy and paste the embed code to your page. If the content isn't in your page yet, then choose \[Map + Content Widget] and copy and paste the embed code to your page.

## Changing the look and feel

### Choosing a different layer provider

![](../../.gitbook/assets/choose-provider.png)

The Map widget uses [Leaflet](https://leafletjs.com) as map rendering service and currently supports various map layer providers. The current out-of-box providers and Open Street Maps, Carto DB, Mapzen and Google Maps.

For example, instead of using the default layer, you can use the Dark Matter theme from CartoDB.

### Fine tuning look and feel

![](../../.gitbook/assets/provider-type-settings.png)

After a provider is selected, you can change the look and feel such as pin colour or map color. Different map providers might have slightly different options.

### Previewing the modified map

![](../../.gitbook/assets/changed-to-dark-matters.png)

The map layer has changed to CartoDB Dark Matters and the pin colour is also changed.

## Overlaying the map on your existing Google Map

![](../../.gitbook/assets/overlay-on-google-map.png)

If you have already got a Google Map running on your site, you can also make the map feature overlay on top of your existing Google Map.

Below is the sample code to

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>JS Bin</title>
</head>
<body>
<!-- Google Map element -->
<div id="map" style="height: 500px"></div>

<!-- Change the inline style to hide the map widget -->
<div class="stackla-widget" data-id="10811" data-hash="579ab61508113" data-ct=""
     data-alias="georgezhao.stackla.com" data-ttl="30" style="width: 100%; overflow: hidden; height: 500px; display: none;"></div>
<script type="text/javascript">
    (function (d, id) {
        if (d.getElementById(id)) return;
        var t = d.createElement('script');
        t.type = 'text/javascript';
        t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js';
        t.id = id;
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t);
    }(document, 'stackla-widget-js'));
</script>
<div class="stackla-widget" data-id="10243" data-hash="577b624a5b11d" data-ct=""
     data-alias="georgezhao.stackla.com" data-ttl="30" style="width: 100%; overflow: hidden;"></div>
<script type="text/javascript">
    (function (d, id) {
        if (d.getElementById(id)) return;
        var t = d.createElement('script');
        t.type = 'text/javascript';
        t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js';
        t.id = id;
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t);
    }(document, 'stackla-widget-js'));
</script>

<script>
    // Custom code to overlay Map widget with Google Map
    function init() {
        if (!window.Stackla) {
            // wait until the Nosto's UGC object is ready
            setTimeout(init, 100);
            return;
        }
        initMap();
    }

    function initMap() {
        var coordinateSydney = {lat: -33.8568, lng: 151.2153};
        var map = new google.maps.Map(document.getElementById('map'), {zoom: 11, center: coordinateSydney});

        var options = {
            /**
             * @param {HTMLElement} dom
             * @param statistics
             * @returns {*}
             */
            decoratePin: function (dom, statistics) {
                // you can access the following statistics before a pin is rendered
                var avgLocation = statistics.avg_location;
                var quantity = statistics.quantity;
                var geohash = statistics.geohash;

                // you can modify or add listeners before a pin is rendered
                return dom;
            }
            // please see the table below for all available options
        };

        // Nosto's UGC code
        var mapWidget = Stackla.WidgetManager.getWidgetInstances()[0];
        mapWidget.bindGoogleMap(map, options);
    }
</script>
<script src="https://maps.googleapis.com/maps/api/js?callback=init"></script>
</body>
</html>
```

## Available Options

| Option Name | Type                                                      | Description                                                                                                                                                                          |
| ----------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| decoratePin | Function (pin:HTMLElement, statistics:Object):HTMLElement | A function provided by the developer to modify the pin DOM before it is rendered. It accepts the pre-generated DOM and developer can return a modified DOM.                          |
| pinOnClick  | Function (geohash:String):void                            | A function provided by the developer to override the on-click behaviour. If this function is provided, the click action on the pin will no longer trigger the Change-Geohash action. |

## Final Demo

```html
<script type="text/javascript">(function (d, id) { if (d.getElementById(id)) return; var t = d.createElement('script'); t.type = 'text/javascript'; t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js'; t.id = id; (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t); }(document, 'stackla-widget-js'));</script>

<script type="text/javascript">(function (d, id) { if (d.getElementById(id)) return; var t = d.createElement('script'); t.type = 'text/javascript'; t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js'; t.id = id; (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t); }(document, 'stackla-widget-js')); </script>

 
<script>function init() { if (!window.Stackla) { setTimeout(init, 100); return; } initMap(); } <div></div> function initMap() { var coordinateSydney = {lat: -33.8568, lng: 151.2153}; var map = new google.maps.Map(document.getElementById('map'), {zoom: 11, center: coordinateSydney}); <div></div> // Nosto's UGC code var mapWidget = Stackla.WidgetManager.getWidgetInstances()[0]; mapWidget.bindGoogleMap(map); }</script>

<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAZmmYJnG5-kRdV95bN-JFTTeOIyV_5Op8&callback=init"></script>
```
