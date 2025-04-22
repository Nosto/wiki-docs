# Tracking UGC on Adobe Analytics

* [Overview](tracking-stackla-on-adobe-analytics.md#overview)
* [Basic Tracking](tracking-stackla-on-adobe-analytics.md#basic-tracking)
  * [Basic Tracking Code](tracking-stackla-on-adobe-analytics.md#basic-tracking-code)
    * [jQuery.getscript Sample Code](tracking-stackla-on-adobe-analytics.md#jquery-getscript-sample-code)
    * [Stackla.loadJS Sample Code](tracking-stackla-on-adobe-analytics.md#stackla-loadjs-sample-code)
  * [Track iFrames Load](tracking-stackla-on-adobe-analytics.md#track-iframes-load)
    * [Sample Parent Page Code](tracking-stackla-on-adobe-analytics.md#sample-parent-page-code)
    * [Sample Code for Widget](tracking-stackla-on-adobe-analytics.md#sample-code-for-widget)
* [Event/Link Tracking](tracking-stackla-on-adobe-analytics.md#event-link-tracking)
  * [Global Widgets Events API](tracking-stackla-on-adobe-analytics.md#global-widgets-events-api)
    * [Example](tracking-stackla-on-adobe-analytics.md#global-widgets-events-api-example)
  * [Adobe Analytic Definitions / Prerequisites](tracking-stackla-on-adobe-analytics.md#adobe-analytic-definitions-prerequisites)
    * [s.tl() Function](tracking-stackla-on-adobe-analytics.md#adobe-analytic-definitions-prerequisites-function)
    * [Structure](tracking-stackla-on-adobe-analytics.md#adobe-analytic-definitions-prerequisites-structure)
    * [Variables](tracking-stackla-on-adobe-analytics.md#adobe-analytic-definitions-prerequisites-variables)
  * [Examples](tracking-stackla-on-adobe-analytics.md#event-link-tracking-example)
    * [UserClick - Track Exit Link](tracking-stackla-on-adobe-analytics.md#userclick-track-exit-link)
    * [userClick - Track Exit Link + Populate Variables](tracking-stackla-on-adobe-analytics.md#userclick-track-exit-link-populate-varaibles)
    * [productActionClick - Track Custom Link](tracking-stackla-on-adobe-analytics.md#productactionclick-track-custom-link)
* [Campaign Tracking](tracking-stackla-on-adobe-analytics.md#campaign-tracking)
  * [Product Tags](tracking-stackla-on-adobe-analytics.md#product-tags)
  * [Custom Campaign Tags](tracking-stackla-on-adobe-analytics.md#custom-campaign-tags)

## Overview

This document provides an overview of the different approaches which can be taken by a client who uses Adobe Analytics as their Web Traffic Analytics tool.

The document outlines the options and level of tracking available within the tool for a client to take advantage of.

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

## Basic Tracking

### Basic Tracking Code

For clients who wish to reference the standard Analytics code directly on their Widget, they can simply use the jQuery call `jQuery.getscript` or the `Stackla.loadJS` call to achieve this.

Both calls will fetch the relevant external JS file and load it whenever the Widget loads, however the jQuery script will load the code in sequential order, whilst the Nosto's UGC call will load all external JS files simultaneously.

#### jQuery.getscript Sample Code

```
$.getScript('ttps://<jsfile>.js')
    .then($.getScript('https://<jsfile>.js'))
    .then($.getScript('https://<jsfile>.js'))
    .then(function () {
        // Do something after the external files are loaded
    });
```

#### Stackla.loadJS Sample Code

```
Stackla.loadJS([
    'https://<jsfile>.js',
    'https://<jsfile>.js',
    'https://<jsfile>.js'
]).then(function() {
    // Do something after the external files are loaded
});
```

**Please Note**: Nosto's UGC renders as an iFrame on the Parent Page upon which it has been embedded. As such, duplicating the same code on both the Parent Page and iFrame may result in double counting within your Adobe Analytics account.

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

### Track iFrames Load

When the Nosto's UGC Widget renders on a page it renders as an iFrame, this means if a client wants, they can use postMessage within the Nosto's UGC Widget and create an event listener on event "message" on the parent page to record if the Widget has in-fact loaded.

The framework of the code that would need to be used is provided below:

#### Sample Parent Page Code

```html
<script>
    function listenMessage(e) {
        if(e.data == "Iframe is loaded"){
            alert(e.data);
            /*Place Adobe Analytics Code*/
        }
    }

    if (window.addEventListener) {
        window.addEventListener("message", listenMessage, false);
    } else {
        window.attachEvent("onmessage", listenMessage);
    }
</script>
```

#### Sample Code for Widget

```
window.top.postMessage('Iframe is loaded','http://url');
```

The above code should be used in conjunction with the Nosto's UGC Javascript API - Inline Tile API Callbacks.

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

## Event/Link Tracking

Nosto's UGC, through it’s Global Widgets Events API, provides an number of events for clients to subscribe to in order to more accurately track user interactions.

### Global Widgets Events API

The Global Widgets Events API is a Javascript API that is only available in Vertical Fluid, Horizontal Fluid, Waterfall, Carousel and Mosaic widgets.

The Event is available via the `window.Stackla.WidgetManager` global variable.

#### Example

```
Stackla.WidgetManager.on('tileExpand', function (e, data) {
    // Maybe you need to filter when you have multiple widgets on the same page.
    if (data.widgetId === 9999) {
        // do something...
    }
});
```

To find out more information around these `events` please use [this guide](../../api-docs/javascript/widgets/reference.md#events)

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

### Adobe Analytic Definitions / Prerequisites

The level of detail / granularity that the client wishes to record in their Adobe Analytics environment will determine whether any pre-configuring is required prior to tracking the Nosto's UGC Global Widgets API events.

Adobe Analytics has three core variables which can be used to track information based upon events at a more granular detail. These variables are:

* Custom Conversion Variables (**eVars**)
* Custom Traffic Variables (**sProps**)
* Success **Events**

For the purpose of the tracking Events from the Nosto's UGC Widget, the Variables eVars and Events will be used.

It is recommended that these configurations are defined within the client’s Adobe Analytics instance before implementing any triggers within Stackla.

#### s.tl() Function

This guide leverages outlines a number of call using the function `s.tl()`. This function has been identified as appropriate for a number of the use-cases relating to tracking Events from a Nosto's UGC Widget and reporting them within Adobe Analytics.

This function is not the only function that can be used by a client, and is more a recommendation versus a prescription. Outlined below is the structure and variables relating to this function which are relevant for the examples in this guide.

#### Structure

```
s.tl(this,linkType,linkName, variableOverrides, doneAction)
```

#### Variables

| Variable                                                                                      | Description                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `this`                                                                                        | The first argument should always be set either to this (default) or true. The argument refers to the object being clicked; when set to "this," it refers to the HREF property of the link. |
| `linkType`                                                                                    |                                                                                                                                                                                            |
| `linkType` has three possible values, depending on the type of link that you want to capture. |                                                                                                                                                                                            |

* File Download - `d`
* Exit Links - `e`
* Custom Links - `o`

\| | `linkName` | This can be any custom value, up to 100 characters. This determines how the link is displayed in the appropriate report. | | `variablesOverrides` | (Optional, pass null if not using) This lets you change variable values for this single call, It is similar to other AppMeasurement libraries. | | `doneAction` | An optional parameter to specify a navigation action to execute after the track link call completes when `useForcedLinkTracking` is enabled. |

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

### Examples

The following examples will generate a trackable event from the Expanded Tile view of the Nosto's UGC Widget that can be passed to Adobe Analytics.

Each of these examples have a series of assumptions which have been made on the Adobe Analytics end. The assumptions will need to be customised to work for a client’s specific use-case.

#### userClick - Track Exit Link

This example leverages Adobe Analytics function `s.tl()`. The structure of this function is listed under the Adobe Analytics Definitions / Prerequisites.

**Assumptions**

Client wants to track whenever a visitor clicks on a `UserName` (and is then sent to an the Source Network) that it tracks as an Exit Link in Adobe Analytics. No `eVars` or Events will be tracked as part of this code.

**Variables**

* `linkType` - Exit Link
* `linkName` - Widget-UserNameClick

**Sample code**

```
Stackla.WidgetManager.on('userClick', function (e, data) {
    s.tl(this,'e','Widget-UserNameClick');
});
```

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

#### userClick - Track Exit Link + Populate Variables

This example leverages Adobe Analytics function `s.tl()`. The structure of this function is listed under the Adobe Analytics Definitions / Prerequisites.

**Assumptions**

Client wants to track whenever a visitor clicks on a UserName (and is then sent to an the Source Network) that it tracks as an Exit Link in Adobe Analytics. Client also wants to append additional information to Event and related eVars.

For the example, the Event has been defined as `event1` and the `eVars` as `eVar45` and `eVar46`

**Variables**

* `linkType` - Exit Link
* `linkName` - Widget-UseNameClick
* `linkTrackEvents` - event1
* `eVar45` - WidgetID
* `eVar46` - TileID

**Sample Code**

```
Stackla.WidgetManager.on('userClick', function (e, data) {
    s.linkTrackVars= s.linkTrackVars+',eVar45,eVar46,events'; s.events='event1'; s.linkTrackEvents='event1'; s.eVar45='WidgetID: ' + data.widgetId; s.eVar46='TileID: ' + data.tileData._id.$id;
            s.tl(this,'e','Widget-UserNameClick');
});
```

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

#### productActionClick - Track Custom Link

This example leverages Adobe Analytics function `s.tl()`. The structure of this function is listed under the Adobe Analytics Definitions / Prerequisites.

**Assumptions**

Client wants to track whenever a visitor clicks on a productActionLink that it tracks as an Custom Link in Adobe Analytics. Client also wants to append additional information to Event and related eVars.

For the example, the Event has been defined as `event1` and the `eVars` as `eVar45` and `eVar46`

**Variables**

* `linkType` - Custom Link
* `linkName` - Widget-UseNameClick
* `linkTrackEvents` - event1
* `eVar45` - WidgetID
* `eVar46` - TileID

**Sample Code**

```
Stackla.WidgetManager.on('productActionClick', function (e, data) {
    s.linkTrackVars= s.linkTrackVars+',eVar45,eVar46,events'; s.events='event1'; s.linkTrackEvents='event1'; s.eVar45='WidgetID: ' + data.widgetId; s.eVar46='TileID: ' + data.tileData._id.$id;
            s.tl(this,'o','Widget-UserNameClick');
});
```

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

## Campaign Tracking

One of the features of the Adobe Analytics tool is the ability to track the flow of a user through different sites and pages within a campaign.

The Campaign functionality enables clients to track conversion of pre-defined campaigns/goals within the tool.

### Product Tags

Nosto's UGC’s Product Tags enable clients to specify a URL and a Call to Action (CTA) which can be rendered as a Social Commerce or ShopSpot output on the Visual UGC Widgets.

For clients which simply want to track all clicks on these output and associate them with Nosto's UGC, the client can simply interest the Campaign ID `cid` to the end of the respective URLs, and then each time this is clicked, Adobe Analytics will be updated.

The structure of the URL will simply be:

```
https://<clientdomain>/<producturl>?cid=<campaignID>
```

This approach works for clients who want to use the same Campaign ID across all Nosto's UGC Widget outputs.

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)

### Custom Campaign Tags

For clients who want to have a more customised Campaign Tag (ie. Because they want to track on a per Widget basis or because they want the Parent Page to influence the Tag), Nosto's UGC’s JS API provides the ability for a client to either fetch variables from the Parent Page or alternatively declare a variable per Widget.

[Back to Top](tracking-stackla-on-adobe-analytics.md#top)
