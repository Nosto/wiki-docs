# Architecture of widgets

**Repositories**

The repositories being utilised across the widget development pipeline include:\


**Templates Repository**

The purpose of this repository is to allow the development of widget templates.

This repository contains the SCSS, TSX and Handlebars templates utilised to create the templates.

The logic here is intentionally separated from framework level code to keep development simple. To manage your widget, we would recommend copying the blankcanvas widget folder and rename it to a folder name suitable for your new widget.

* [https://github.com/stackla/stackla-widget-templates](https://github.com/stackla/stackla-widget-templates)

**Widget Utilities Repository**

The purpose of this repository is to create utilities that engineers can maintain and share across their projects. We welcome any contributions to this repository for tools that may be helpful in the development of widgets.

There is also some widget configuration based logic utilised here to allow the framework to hook into the widget templates and respect given properties, for instance - when colors are defined in the widget settings screen of my.stackla.com, they should be managed in the widget-utils repository.

* [https://github.com/Stackla/widget-utils](https://github.com/Stackla/widget-utils)

**Shadow Root**

A shadow root is defined for the widget, which enables the widget to remain isolated from the rest of the DOM.&#x20;

The Shadow Root is accessible for mutation from external JS manipulation if required, as its an **'open'** Shadow Root.

![](<../../../../../.gitbook/assets/image (16).png>)

To mutate the shadow root, users can either utilise the prebuilt **SDK** or manually target the ugc-widget div.&#x20;

Best practice would ensure mutation logic is isolated in the widget code.

**Server Side Rendering**

The initial load of the widget is server side rendered, along with future tile retrieval.

An initial request is made to the widget server which retrieves the configuration associated with the widget, the HTML rendered from the Handlebars templates, and all of the JS & CSS.

**Tiles Rendering**

Depending on the widget you utilise, tiles are automatically rendered on the screen based on a predetermined algorithm.

<figure><img src="../../../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Each tile is rendered using a **.ugc-tile** div. These elements can be mutated as necessary.

Tiles are rendered by utilising the [tiles service](https://github.com/Stackla/widget-utils/blob/master/src/types/services/tiles.service.ts), and users can access the tiles service by utilising sdk.tiles.

The amount of tiles loaded at a given time can be updated by utilising **sdk.tiles.setVisibleTilesCount()**.&#x20;

Before utilising this method, please be sure that there are no side effects being performed by the base boilerplate code, otherwise you may find that your visibleTilesCount is mutated and no longer matches the value you are looking for.



**Advanced Usage**

**Globals**

<figure><img src="../../../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Globals are located in the window.ugc namespace - this is where all widgets can be accessed if required.

To access a widget's SDK outside of the widget tsx, a user can utilise **window.ugc.getWidgetBySelector().sdk.**&#x20;

**Placement**

What you see on the screen is the placement. The placement contains the widget, and any associated configuration responsible for the display of the widget.

The [placement](https://github.com/Stackla/widget-utils/blob/master/src/types/core/placement.ts) can be accessed with **sdk.getPlacement().**

**Events**

Events are utilised to enable the interaction between different decoupled components. For instance, when the Load More button is clicked, a moreLoad event is invoked, which can be listened to by multiple listeners to trigger side effects.

The [events service ](https://github.com/Stackla/widget-utils/blob/master/src/types/services/event.service.ts)can be accessed with **sdk.getEvents()**













