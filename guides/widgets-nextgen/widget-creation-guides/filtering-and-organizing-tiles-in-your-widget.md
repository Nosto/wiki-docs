# Filtering and Organizing Tiles in Your Widget

This guide will help you set up your widget so users can **filter content** (tiles) and **organize how it’s displayed**. It assumes you’re using a **widget editor** with Layout, Tile, CSS, and JS boxes.



**Prerequisites:**

Any widget template, we recommend Grid widget for this one but please create a widget with a default filter already selected.

***

### **1. Layout Box – Adding Filter Buttons**

The **Layout Box** is where you define the buttons or controls users click to filter or organize content.

**Example:**

```html
<div class="stackla-links" style="display: flex; gap: 10px;">
  <a href="#!" id="154374">For Her</a> |
  <a href="#!" id="154375">For Him</a> |
  <a href="#!" id="154376">For Teens</a> |
  <a href="#!" id="154377">For Kids</a> |
  <a href="#!" id="154378">For Pets</a>
</div>

<div class="ugc-tiles grid grid-inline" aria-label="Content grid">
    {{#tiles}}
    {{>tpl-tile options=../options}}
    {{/tiles}}
</div>

<load-more widgetId="{{options.wid}}" aria-label="Load more content" />
```

**What this does:**

* `.stackla-links` → Buttons users click to filter tiles.
* `.ugc-tiles` → Grid where tiles will appear.
* `<load-more>` → Button to load more content if needed.

**Tip:** You can add as many buttons as you like for different filters, brands, or categories.

***

### **2. Tile Box – How Each Tile Appears**

The **Tile Box** controls the **look of each tile**. Tiles can be images or videos, and you can show icons for shoppable content, Instagram Reels, or other indicators.

**Example:**

```html
{{#tile class="grid-item"}}
  <div class="tile" data-background-image="{{image}}">
    <img style="display: none;" src="{{image}}" alt="{{name}}" />
    <div class="icon-section">
      {{#ifHasProductTags this}}
        <div class="shopping-icon icon-products" aria-label="Shoppable content"></div>
      {{/ifHasProductTags}}
      <div class="network-icon icon-{{source}}" aria-label="Content from {{source}}"></div>
    </div>
    <tile-tags widgetId="{{options.wid}}" tile-id="{{id}}" variant="dark" mode="swiper" context="grid-inline" aria-label="Content tags"></tile-tags>
    <shopspot-icon widgetId="{{options.wid}}" tile-id={{id}} aria-label="Shopping options" />
  </div>
{{/tile}}
```

**What this does:**

* Displays images or video tiles.
* Shows icons for shoppable products or Instagram Reels.
* Displays content tags and shopspots.

**Tip:** Customize icons or tags to fit your style.

***

### **3. CSS Box – Organizing Tiles Visually**

The **CSS Box** controls **how your tiles are laid out** and how the filter buttons look.

**Example CSS:**

```css
.stackla-links a {
  text-decoration: none;
  color: #333;
  font-weight: bold;
  cursor: pointer;
  margin-right: 8px;
}

.stackla-links a:hover {
  color: #007bff;
}

.ugc-tiles {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
}

.tile {
  position: relative;
  border-radius: 8px;
  overflow: hidden;
}
```

**Tip:** Use CSS to create **different grid layouts** (e.g., single column, multi-column, masonry style) so users can organize content visually.

***

### **4. JS Box – Making Filters Work**

The **JS Box** makes your filters interactive and allows users to **organize tiles in different ways**. You can filter by:

* **Filter ID** – Predefined filters (like “For Her” or “For Him”)
* **Tags** – Content tags like “summer” or “holiday”
* **Brand** – Tiles from a specific brand
* **Category** – E.g., sports, fashion
* **Text Search** – Search by text or keyword
* **Tag Group** – Regional or group-based filtering

**Example JS for Filter Buttons:**

```js
// Load the grid template
sdk.loadTemplate('grid');

// Make filter buttons clickable
sdk.querySelectorAll(".stackla-links a").forEach(item => {
  item.onclick = () => {
    const filterId = item.getAttribute('id');
    sdk.searchTilesByFilterId(filterId, true); // true = replace current tiles
  };
});
```

**Example: Other Ways to Filter**

```js
// Search by text
sdk.searchTiles('summer');

// Search by tags
sdk.searchTilesByTags('holiday,vacation');

// Search by brand
sdk.searchTilesByBrand('Nike');

// Search by categories
sdk.searchTilesByCategories('sports,fashion');

// Search by tag group
sdk.searchTilesByTagGroup('au'); // e.g., Australia
```

**Tip:** You can create buttons or dropdowns for **different ways to organize tiles**, not just filter IDs.

***

### **5. Organizing Tiles Dynamically**

You can let users **choose how to organize content**:

* **Filter by tags** → Let users pick multiple tags.
* **Mix filters** → Users can combine brand + category + tag.

**Example: Combining Filters**

```js
// Show Nike products in sports category
sdk.searchTilesByBrand('Nike');
sdk.searchTilesByCategories('sports', false); // false = append to current results
```

***

### **6. Extra Features**

* **Highlight active filter**:

```js
sdk.querySelectorAll(".stackla-links a").forEach(item => {
  item.onclick = () => {
    sdk.searchTilesByFilterId(item.getAttribute('id'), true);
    // highlight active button
    sdk.querySelectorAll(".stackla-links a").forEach(i => i.style.fontWeight = 'normal');
    item.style.fontWeight = 'bold';
  };
});
```

***

### **Summary**

1. **Layout Box:** Create filter buttons and grid container.
2. **Tile Box:** Decide how each tile looks (image/video, icons, tags).
3. **CSS Box:** Style buttons, tiles, and grid layout.
4. **JS Box:** Make filters work and allow users to organize tiles in different ways.

**Result:** Users can interact with your widget to **filter content, view by category/brand/tag, and organize tiles dynamically**, all without needing to write complex code.



### **Result**

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
