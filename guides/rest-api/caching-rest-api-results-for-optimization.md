# Caching REST API results for optimization

## Overview

When data fetched from Nosto's UGC via API is required for every page view, it can be expensive to request every impression. Often the data does not need to be the latest, especially if it's known that the data does not change often. In this guide, we will explore methods to reduce API consumption (reducing the risk of exceeding the API rate limit), while utilizing the data at the same time.

By using the cache method, we will end up having a faster web response and REST API's rate limits will not limit us to serve our busy web page to clients.

In this guide, we will use Memcached in a demo PHP application to implement a caching strategy for Filter data.

### A note on cache methods

There are several cache methods that we can use and your mileage will certainly vary.

Most languages and frameworks have similar patterns for caching locally (e.g. local files or RAM such as [PHP-APC](http://php.net/manual/en/book.apc.php) or [node-cache](https://www.npmjs.com/package/node-cache)), as well as drivers for distributed caching systems such as [memcached](http://memcached.org). Further to this, edge server caching applications such as [Varnish](https://www.varnish-cache.org) can cache the result before it even hits your web server, or web server extensions can add the capability to Apache or nginx.

In each of these, the pattern is similar: rather than hitting the Nosto's UGC API every time data is required from a resource, store the response for some time in a cache and use the cached version until that period expires for all requests.

In the case of local cache, the data is stored on each web server. While good for single-server implementations, once you exceed one application server, and since each server will have its own cache, you run the risk of using different versions in different requests.

Distributed cache systems such as memcached resolve the above problem and allow a single cache to be accessed from multiple sources. For example, each one of your 5 application servers can have their application access the identical source for the cached data.

If you wish to step to a higher level, you can store the web server response itself in a cache and serve that to visitors via your web server or reverse proxy of some kind.

## Key Concepts

### memcached

Memcached is a general-purpose distributed memory caching system.

### Filters API

Filters API provides an endpoint to obtain the filters that are available in the Stack.

### Example Application

In this application, we will utilize memcached to cache the response of Nosto's UGC API in raw format and generate an output of it.

## The Fun Part

In this guide we will do the following:

* Install and configure memcached
* Generate uncached requests
* Generate cached requests

### Install and configure memcached

In this guide we won't be going through installing it as it comes in many variants, however, if you're not familiar there is great info on the Memcached[ Wiki](https://code.google.com/p/memcached/wiki/NewStart) and [PHP Manual](http://php.net/manual/en/book.memcached.php).

### Generate uncached requests

```

<?php
// Fetch filters from Nosto's UGC REST API
$filters = getFiltersFromStackla();
echo '<select name="filters">';
foreach ($filters as $id => $name) {
    echo "<option value=\"{$id}\">{$name}</option>";
}
echo '</select>';
// The End
```

Execute the above code several times, and you will notice that every request generates hits on the Nosto's UGC API, and the API console will indicate that there are now fewer requests available.

### Generate cached requests

#### Note on getFiltersFromStackla()

Before we do anything, we assume that there is a function named `getFiltersFromStackla` that retrieves the raw JSON from the Stack and processes it into an associative array (id=>name). Details on how to acquire the Filters are covered in [this guide](../onsite-widgets/render-widget-filters-dynamically.md), although the effort to wrap the task in the `getFiltersFromStackla` is left as a separate exercise.

#### Connect to and query Memcached

The following code is a sample of how to connect to the memcached server.

```
$mc = new Memcache();
// Modify this for your server
$mcHost = 'localhost';
$mcPort = 11211;
$mcPresist = true;
$mcWeight = 1;
$mcTimeout = 1;

// Add a Memcached server to the connection pool
$mc->addServer($mcHost, $mcPort, $mcPresist, $mcWeight, $mcTimeout);

// Connect to Memcached server
$mc->connect($mcHost, $mcPort);
```

Now that we have a connection, we can query for particular data, and if not found, store the data itself. memcached is a key-value store, so we will need a unique key to store the data.

```
if ($mc->get('filters')) {
    // Get cached value
    $filters = $mc->get('filters');
} else {
    // Fetch filters from Nosto's UGC REST API
    $filters = getFiltersFromStackla();
    // Cache filters for the next 30 seconds
    $mc->set('filters', $filters, time() + 30);
}
```

```

<?php
$mHost = 'localhost';
$mPort = 11211;
$mPresist = TRUE;
$mWeight = 1;
$mTimeout = 1;
$m = new Memcache();
// Add a Memcached server to the connection pool
$m->addServer ( $mHost, $mPort, $mPresist, $mWeight, $mTimeout);
// Connect to the Memcached server
$m->connect($mHost, $mPort);
// Check if the filters result is in Memcached
if ($m->get('filters')) {
    // Get cached value
    $filters = $m->get('filters');
} else {
    // Fetch filters from Nosto's UGC REST API
    $filters = getFiltersFromStackla();
    // Cache filters for 30 seconds minute
    $m->set('filters', $filters, time() + 30);
}
echo '<select name="filters">';
foreach ($filters as $id => $name) {
    echo "<option value=\"{$id}\">{$name}</option>";
}
echo '</select>';
// The End
```

After executing this code, you will notice that the first request reduces the allowed requests in the console, however for the following 30 seconds, subsequent hits do not. This is the effect of the cache.

## Summary and Next Steps

This example can be further expanded with the following activities:

* Caching other Nosto's UGC endpoints
* Caching Filter content after dynamically changing Filter settings (personalized content)
