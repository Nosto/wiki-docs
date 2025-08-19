# Creating a starter project

## Starter project activity

This project has been created with the sole purpose of:

1. Allowing you to see what is possible with widgets
2. A space for you to follow the activity provided to learn about how you can customise widgets with ease

### What you will learn

1. How to adjust styles for inline tiles & expanded tiles
2. How to adjust templates for inline tiles & expanded tiles.
3. How to hook into emitted events
4. How to apply dynamic css variables

### Getting started with the activity

* You will notice this widget is bouncing. Please stop the circle from bouncing.
* Why is the widget in a circle? Please remove the circle and ensure it is in a grid format, covering the entire screen.
* The border of the circle is based on the mood given to the method. Please update it so that it is using 'nosto' mood with a pink border to represent Nosto.
* Every time I expand a tile, a dog appears. Please change this to the actual tile image.
* Every time an event is emitted, a toast message appears, please remove this.

A successful attempt at this activity will be something like the following.&#x20;

<figure><img src="../../../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

**Answers:**

* To stop the circle from bouncing, remove `@keyframes bounce { }` in extras.scss
* To remove the circle, remove the `border-radius` property in `extras.scss`
* To update the border color to the Nosto mood, update the `"--my-mood-border"` variable in the widget.tsx file.
* To change the dog image to the actual tile image, update the `tile.template` file to instead use the `tile.image` property.
* To remove the toast message, remove the eventListener in widget.tsx

