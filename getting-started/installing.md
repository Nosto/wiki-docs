# Installing

**Plugin Installation**

For the easiest and safest installation, we highly recommend installing the plugin directly from the official **Shopware Store**. This ensures you always receive stable, tested versions and benefit from simple, one-click updates.

**Advanced Installation (from GitHub)**

For developers or advanced users, you can also install the plugin directly from our GitHub repository: `https://github.com/Nosto/nosto-shopware6`

If you choose this method, you have two options:

**1. From Releases (Preferred GitHub Method)**

We suggest downloading a stable, tagged version from our Releases page: `https://github.com/Nosto/nosto-shopware6/releases`

Please be sure to download the plugin version that matches your Shopware version:

* Plugin Version 6.x.x is for Shopware 6.7
* Plugin Version 5.x.x is for Shopware 6.6
* Plugin Version 3.x.x is for Shopware 6.5

**2. From Branches**

You can also install directly from a branch. You must use the correct branch for your Shopware environment:

* `develop` branch is for Shopware 6.7
* `v5` branch is for Shopware 6.6
* `v3` branch is for Shopware 6.5

***

**Note:** The plugin has the embedded dependency of [Nosto Job Scheduler](https://github.com/Nosto/shopware6-job-scheduler). It's delivered with the plugin sources.

***

**Configuration for Subdirectory Installations and using Nosto Monitoring**

If your Shopware project is located inside a subdirectory (e.g., `/shop/`), you must ensure that the following redirect rules are configured on your web server. This is necessary to properly handle requests to `/nosto-monitoring`.

_(**Note:** "shop" in the examples below should be replaced with the name of your specific subdirectory.)_

**For Apache:**

Add the following lines to your `.htaccess` file, located in the project root (the parent directory of `shop`). These rules should be placed inside the `<IfModule mod_rewrite.c>` block:

```
RewriteRule ^nosto-monitoring/?$ /shop/nosto-monitoring/ [R=308,L]
RewriteRule ^nosto-monitoring/(.*)$ /shop/nosto-monitoring/$1 [R=308,L]
```

Please make sure that:

* Other existing rules do not prevent these new rules from being applied.
* The Apache module `mod_rewrite` is enabled.
* `AllowOverride All` is set in the corresponding `<Directory>` block in your Apache configuration. Otherwise, `.htaccess` rules will not take effect.

**For Nginx:**

Add the equivalent rules to your server configuration (e.g., `/etc/nginx/sites-available/your-site.conf`) inside the main `server {}` block:

```
location ~ ^/nosto-monitoring/?$ {
    return 308 /shop/nosto-monitoring/;
}
location ~ ^/nosto-monitoring/(.*)$ {
    return 308 /shop/nosto-monitoring/$1;
}
```

These redirects ensure that any request to `/nosto-monitoring` is properly redirected to `/shop/nosto-monitoring/`. Please be aware that you may need other rewrite rules to handle your store's general operation from a subdirectory.
