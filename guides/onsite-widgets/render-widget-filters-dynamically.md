# Render Widget filters dynamically

{% hint style="warning" %}
You are reading the Classic Widget Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../widgets-nextgen/).

**Note: This feature is unique to Classic widgets**
{% endhint %}

## Overview

When Nosto's UGC widgets are shown, often a custom menu will be built. The menu will drive the Filter-changing in the widget and often perform other functions too, not necessarily related to Stackla.

In this example we will investigate how Nosto's UGC [REST API](https://github.com/Stackla/docs/blob/master/docs/api-docs/rest/README.md) can be consumed to grab the data related to Filters from the Stack.

## Key concepts

### Widget

A Nosto's UGC Widget will be created to demonstrate the use of the [JavaScript API](../../api-docs/javascript/) to switch the filters being displayed.

### Filter

Nosto's UGC Filters drive the content being displayed in the Widget, so they are a key resource used for the menu. The [REST API](https://github.com/Stackla/docs/blob/master/docs/api-docs/rest/README.md) will be used to grab them from the Stack.

### REST API

The [REST API](https://github.com/Stackla/docs/blob/master/docs/api-docs/rest/README.md) is used to acquire the Filter resources from the Stack. In other words, we will use it to read which Filters the Stack has configured.

### JavaScript API

The [JavaScript API](../../api-docs/javascript/) is will be used to talk to the Widget on the site and get it to switch the Filter being displayed.

### Example Application

To demonstrate the reading of data and rendering of the menu we will create a new PHP application. The application will effectively consume the Filters resource from the Stack and according to its data, render a menu. The menu itself will be a simple rendering of the Filter data that we receive.

You will need to have a working web server active and Internet connectivity to https://api.stackla.com/ from the server to be able to consume the [REST API](https://github.com/Stackla/docs/blob/master/docs/api-docs/rest/README.md).

We are also assuming that the Stack already has content in it which you will need to tag, as explained later on.

## The Fun Part

### Getting Started

In this example we will need to do the following:

1. Configure the relevant Filters
2. Create an application to grab the Filter data from Nosto's UGC

### Configure Filters

For this example, we will have three filters to choose from: Latest, Twitter and Instagram. Remove the other Filters, or if you're starting from scratch create them so that they sort by Latest, with Twitter and Instagram filtering by the Twitter and Instagram networks respectively.

Because the filters themselves will give us a different range of content, we will not be using any Tags to achieve this further.

### Create the Example Application

Grabbing Filter data from Nosto's UGC is a really simple task. In fact, all it really involves is consuming a RESTful resource: filters. In other words, you will need to perform a HTTP GET operation on `https://api.stackla.com/api/filters` (with some minor mods, as outlined in the following section). The URL can be retargeted towards your specific stack, which always has an alias as a subdomain on the `stackla.com` domain. For example, stack `myawesomestack` will have an alias as https://myawesomestack.stackla.com/, no matter what other URLs you may use to access.

**Note:** if you choose to use `api.stackla.com` instead, you will need to pass your Stack name as a `stack` query parameter, e.g. `https://api.stackla.com/api/filters?stack=myawesomestack`

The following code will simply render a HTML page, and use some inline PHP (please don't judge too harshily) to render a menu as a HTML list.

#### Grabbing RESTfully

The following code can be used to grab the JSON data from the API. XML data can be acquired by passing the "xml" extension next to the filters resource.

**A note about** [**file\_get\_contents**](http://php.net/manual/en/function.file-get-contents.php)**:**

If the below request was to fail (4xx, 5xx and not 200), file\_get\_contents will not give you the body of the message. This will make it hard to detect or debug errors returned by the API. For this example, we will use file\_get\_contents as it's the easiest to demo, but you may instead consider using REST libraries in your PHP framework or [cURL](http://au2.php.net/manual/en/book.curl.php).

```
$requestUrl = 'https://api.stackla.com/api/filters.json?access_token=1234abcd5678efgh9012ijkl3456mnop&stack=myawesomestack';
$filtersData = file_get_contents($requestUrl);

// If data is returned, convert it into an object
if($filtersData) {
    // If data is returned, convert it into an object
    $filters = json_decode($filtersData)->data;
} else {
    // We got no data -- panic and create an empty array.
    $filters = array();
}
```

Once we have some data in the `filters` object we can loop through them and output in a menu such as a select dropdown.

```
<select onchange="navigateByFilter(this.value);">
    <?php foreach($filters as $filter): ?>
    <option value="<?=$filter->id?>"><?=$filter->name?></option>
    <?php endforeach; ?>
</select>
```

#### Changing Filters via embed code

A Widget's set Filter can be overwritten by the HTML on the embed code by using the `data-filter` attribute. In the code for the `div` tag, set the attribute with the ID of the Filter. For example, this code sets the Filter ID to be 9999.

```html
<div class='stacklafw' data-id='1234' data-filter="9999" data-hash='aabbccddeeff' data-ct='' data-alias='stackla-developer.stackla.com' data-ttl="30"></div>
<script type='text/javascript'>
    (function (d, id) {
        if (d.getElementById(id)) return;
        var t = d.createElement('script');
        t.type = 'text/javascript';
        t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js';
        t.id = id;
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t);
    }(document, 'stacklafw-js'));
</script>
```

#### Changing Filters via JavaScript

When a Fluid Widget is on the page, a `StacklaFluidWidget` object exists that can be referenced. If the widget is the only instance of the specific widget on the page, we can use the `changeFilter` method with the Widget's ID to switch the Filter by the Filter's ID.

```
var widgetId = 1234;
var filterId = 5678;
StacklaFluidWidget.changeFilter(widgetId, filterId);
```

#### site.php

This is the listing of all of the above combined into one file to serve.

```php
<?php
// This will be your API Key (see API console if you're not sure)
$apiKey = '1234abcd5678efgh9012ijkl3456mnop';

// This will be your Stack Name
$stackName = 'myawesomestack';
$stackUrl = 'https://api.stackla.com/';
$apiUri = 'api/filters';

// The REST endpoint and query URL will be at the following URL.
// Opening the full URL in a web browser should already yield a results to
// demonstrate the use. Note that Visual UGC returns JSON by default. If we
// were to require XML, we can add the extension to the $apiUri above,
// e.g. $apiUri = 'api/filters.xml'
// The final URL should be something like:
//     https://api.stackla.com/api/filters?access_token=1234abcd5678efgh9012ijkl3456mnop&stack=myawesomestack
$requestUrl = $stackUrl . $apiUri . '?access_token=' . $apiKey . '&stack=' . $stackName;

// Note about file_get_contents:
// If the below request was to fail (4xx, 5xx and not 200), file_get_contents
// will not give you the data. This will make it hard to detect errors returned
// by the API. For this example, we will use file_get_contents as it's the
// easiest to demo.
$filtersData = file_get_contents($requestUrl);

if($filtersData) {
    // If data is returned, convert it into an object
    $filters = json_decode($filtersData)->data;
} else {
    // We got no data -- panic and create an empty array.
    $filters = array();
}
?>
<!doctype html>
<html>
<head>
    <title>My Awesome Stack</title>
    <script>
        function navigateByFilter(filterId) {
            // Set the following widgetId with the correct ID of the widget being used.
            var widgetId = '1234';
            StacklaFluidWidget.changeFilter('1234', filterId);
        }
    </script>
</head>
<body>
    <h1>Welcome to the awesomest Stack on the planet!</h1>
    <p>To change between my family photos and the different locations I've been
        to, use the dropdown menu below.</p>
    <select onchange="navigateByFilter(this.value);">
        <?php foreach($filters as $filter): ?>
        <option value="<?=$filter->id?>"><?=$filter->name?></option>
        <?php endforeach; ?>
    </select>

    <div id="stackla-widget">
        <!-- BEGIN: Your widget code -->
        <!-- Paste your widget code in the area below. Keep in mind your data-id attribute -->
        <div class='stacklafw' data-id='1234' data-hash='aabbccddeeff' data-ct='' data-alias='stackla-developer.stackla.com' data-ttl="30" ></div>
        <script type='text/javascript'>
            (function (d, id) {
                if (d.getElementById(id)) return;
                var t = d.createElement('script');
                t.type = 'text/javascript';
                t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js';
                t.id = id;
                (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(t);
            }(document, 'stacklafw-js'));
        </script>
        <!-- END: Your widget code -->
    </div>
</body>
</html>
```

## Summary and Next Steps

This was a very basic example to demonstrate some use of the REST API and Widgets JavaScript API on the same page. You can expand this example to:

* Build a better menu for switching
* Use the public display options or naming pattern of Filters to control whether the Filter should be available for display
* Use a naming pattern for the filters
