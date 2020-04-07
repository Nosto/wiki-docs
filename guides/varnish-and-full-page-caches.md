# Varnish & Full-Page Caches

If you use Varnish or another full-page cache mechanism, you must exclude certain Nosto blocks \(hole punching\).

Nosto adds query parameters to the product URLs for tracking purposes. You may encounter a cache-miss when query parameters are appended to the URL. We recommend [modifying your VCL definition to automatically strip out the query parameters added by Nosto](https://help.nosto.com/manuals/how-can-i-exclude-the-nosto-query-parameter-from-my-varnish-vcl-rules).

## Magento Community Edition

Magento CE does not ship with a built-in FPC mechanism and you **must** manually exclude two Nosto blocks. Failure to do so may result in sensitive information leaking across browser sessions.

* [Cart Block](https://github.com/Nosto/nosto-magento/blob/3.5.1/app/design/frontend/base/default/layout/nostotagging.xml#L57)
* [Customer Block](https://github.com/Nosto/nosto-magento/blob/3.5.1/app/design/frontend/base/default/layout/nostotagging.xml#L62)

> **Note:** The plugin uses a feature called hashed cookie identifiers \(HCID\) to safeguard against the leakage of customer information on Nosto's end.

## Magento Enterprise Edition

If you are using Magento EE's built-in FPC mechanism, you need not worry about hole punching as hole-punching rules are auto-configured.

