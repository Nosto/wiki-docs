# Repository Structure

{% hint style="warning" %}
You are reading the NextGen Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../../../onsite-widgets/) here.

**Note: This feature is unique to NextGen widgets**
{% endhint %}

**Stackla Widget Templates repo**

The Stackla Widget Templates repository is organised with the following structure

**/widgets**

The widgets folder is where all widgets, and widget related code is contained. This is where you will be doing a majority of your work.

Each subfolder in the widgets folder represents a widget. Except for the styles folder, which is where all shared widget styles are contained.

When developing a widget, you can copy a simple widget to begin with, i.e. grid folder, and make tweaks as you go to develop your dream widget.

The basic composition of a widget looks like this:

\
![](<../../../../../.gitbook/assets/image (11).png>)

* widget.scss - This is the root styles file, where all necessary style changes will be made. You can follow SCSS conventions to import other files as required.
* layout.hbs, tile.hbs are utilised for the server side rendered HTML associated with the tiles.
* widget.tsx - This is the root TS file, where all necessary widget code is integrated.

**/packages/widget-utils**

This utilities package is where we handle most of the complexities for the developers, so they don't have to.

In the case that you have changes that you'd like to recommend, please do open a pull request to help us improve our product and the experience for other engineers.

<div align="left"><figure><img src="../../../../../.gitbook/assets/image (12).png" alt="" width="254"><figcaption></figcaption></figure></div>
