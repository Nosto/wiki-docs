# How to change the contents of an expanded tile

This guide will explain how to customize the content displayed within an expanded tile in Nosto UGC NextGen widgets.

## For IDE Users:

As an IDE user, you will directly interact with the widget's codebase, primarily within the widgets/blankcanvas folder. Your main focus will be on widget.tsx for logic, layout.hbs or tile.hbs for HTML structure, and widget.scss for styling.

1. Understanding the Expanded Tile Structure

The expanded tile's content is rendered through the expanded-tiles web component. A sample template is located in samples/expanded-tile.template.tsx. This template dictates what information from the tile is displayed when it expands. When a tile is expanded, the SDK triggers the EVENT\_TILE\_EXPAND event. The expanded-tiles component then renders the content based on its internal logic and the provided template.

2. Modifying the Expanded Tile Template

To change the content, you'll need to override the default expanded-tiles template.

```tsx
// File: widget.tsx

import { ISdk, loadWidget } from "@stackla/widget-utils";
import { config } from "./config";
import { ExpandedTiles } from "./expanded-tile.template"; // Import your custom template

declare const sdk: ISdk;

loadWidget(sdk, {
  config: {
    ...config,
  },
  templates: {
    "expanded-tiles": { // Specify the component you want to override
      template: ExpandedTiles, // Provide your custom template function
    },
  },
});
```

(Create this file if it doesn't exist, and copy the content from samples/expanded-tile.template.tsx as a starting point.)

Within this file, you have full control over the HTML structure of the expanded tile. You can add, remove, or reorder elements to suit your needs.

Here's an example of how you might modify the ExpandedTile function to add a custom header and reorder some elements:

````tsx
/// File: blankcanvas/expanded-tile.template.tsx

import { ISdk, Tile } from "@stackla/widget-utils/types";
import { createElement, createFragment } from "@stackla/widget-utils/jsx";
import {
  VideoContainer,
  VideoErrorFallbackTemplate,
  ExpandedTileProps,
  ShopspotProps
} from "@stackla/widget-utils/components";

declare const sdk: ISdk;

export function ExpandedTile({ tile }: ExpandedTileProps) {
  const { show_shopspots, show_products, show_tags, show_sharing, show_caption, show_timestamp } =
    sdk.getExpandedTileConfig();

  const shopspotEnabled = sdk.isComponentLoaded("shopspots") && show_shopspots && !!tile.hotspots?.length;
  const productsEnabled = sdk.isComponentLoaded("products") && show_products && !!tile.tags_extended?.length;
  const tagsEnabled = show_tags;
  const sharingToolsEnabled = show_sharing;

  const parent = sdk.getNodeId();

  return (
    <>
      <div class="expanded-tile-wrapper">
        <a class="exit" href="#">
          <span class="widget-icon close-white"></span>
        </a>
        <BackArrowIcon /> {/* Assuming you've included BackArrowIcon from the sample */}
        <div class="swiper swiper-expanded">
          <div class="swiper-wrapper ugc-tiles">
            {/* The main tile content structure starts here */}
            <div class="ugc-tile swiper-slide" data-id={tile.id}>
              <div class="panel">
                <div class="panel-left">
                  <IconSection tile={tile} productsEnabled={productsEnabled} />
                  <div class="image-wrapper">
                    <div class="image-wrapper-inner">
                      {tile.media === "video" ? (
                        <>
                          <VideoContainer shopspotEnabled={shopspotEnabled} tile={tile} parent={parent} />
                          <VideoErrorFallbackTemplate tile={tile} />
                        </>
                      ) : tile.media === "image" ? (
                        <ImageTemplate tile={tile} image={tile.image} shopspotEnabled={shopspotEnabled} parent={parent} />
                      ) : tile.media === "text" ? (
                        <span class="content-text">{tile.message}</span>
                      ) : tile.media === "html" ? (
                        <span class="content-html">{tile.html}</span>
                      ) : (
                        <></>
                      )}
                    </div>
                  </div>
                </div>
                <div class="panel-right">
                  <div class="panel-right-wrapper">
                    <div class="content-wrapper">
                      <div class="content-inner-wrapper">
                        {/* --- Custom Header Section --- */}
                        <div class="custom-expanded-header">
                          <h3>Expanded Tile Details</h3>
                          {tile.name && <p>User: {tile.name}</p>}
                          {tile.source_created_at && (
                            <p>
                              Date: <time-phrase source-created-at={tile.source_created_at}></time-phrase>
                            </p>
                          )}
                        </div>
                        {/* --- End Custom Header Section --- */}

                        <tile-content
                          tileId={tile.id}
                          render-share-menu={sharingToolsEnabled}
                          render-caption={show_caption}
                          render-timephrase={show_timestamp}
                        />
                        {tagsEnabled && (
                          <tile-tags tile-id={tile.id} mode="swiper" context="expanded" navigation-arrows="true" />
                        )}
                        {productsEnabled && (
                          <>
                            <ugc-products parent={parent} tile-id={tile.id} />
                          </>
                        )}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            {/* ... other swiper slides if needed */}
          </div>
          {/* Swiper navigation buttons (if enabled) */}
          <div class="swiper-expanded-button-prev swiper-button-prev btn-lg" style={{ display: sdk.getExpandedTileConfig().show_nav ? "flex" : "none" }}>
            <span class="chevron-left" alt="Previous arrow" />
          </div>
          <div class="swiper-expanded-button-next swiper-button-next btn-lg" style={{ display: sdk.getExpandedTileConfig().show_nav ? "flex" : "none" }}>
            <span class="chevron-right" alt="Next arrow" />
          </div>
        </div>
      </div>
    </>
  );
}

// Ensure you also include these helper functions from the original expanded-tile.template.tsx
function BackArrowIcon() {
  return (
    <a class="back" href="#">
      <span class="widget-icon back-arrow"></span>
    </a>
  );
}

export function IconSection({ tile, productsEnabled }: { tile: Tile; productsEnabled: boolean }) {
  const topSectionIconContent = [];
  const bottomSectionIconContent = [];

  if (tile.attrs?.includes("instagram.reel")) {
    topSectionIconContent.push(<div class="content-icon icon-reel"></div>);
  } else if (tile.attrs?.includes("youtube.short")) {
    topSectionIconContent.push(<div class="content-icon icon-youtube-short"></div>);
  }
  if (productsEnabled) {
    topSectionIconContent.push(<div class="shopping-icon icon-products"></div>);
  }

  bottomSectionIconContent.push(<div class={`network-icon icon-${tile.source}`}></div>);

  return (
    <div class="icon-section">
      <div class="top-section">{...topSectionIconContent}</div>
      <div class="bottom-section">{...bottomSectionIconContent}</div>
    </div>
  );
}

export function ShopSpotTemplate({ shopspotEnabled, parent, tileId }: ShopspotProps) {
  return shopspotEnabled ? (
    <>
      <shopspot-icon parent={parent} mode="expanded" tile-id={tileId} />
    </>
  ) : (
    <></>
  );
}

export function ImageTemplate({
  tile,
  image,
  shopspotEnabled = false,
  parent
}: {
  tile: Tile;
  image: string;
  shopspotEnabled?: boolean;
  parent?: string;
}) {
  return image ? (
    <>
      <div class="image-filler blurred" style={{ "background-image": `url('${image}')` }}></div>
      <div class="image">
        {shopspotEnabled ? (
          <ShopSpotTemplate shopspotEnabled={shopspotEnabled} parent={parent} tileId={tile.id} />
        ) : (
          <></>
        )}
        <img class="image-element" src={image} loading="lazy" alt={tile.description || "Image"} />
      </div>
    </>
  ) : (
    <></>
  );
} ```

3. Styling the Expanded Tile
You can apply custom styles to your expanded tile within widget.scss. Remember that all styles are within the shadow DOM.

File: blankcanvas/widget.scss

SCSS
```scss
@use "styles" as gridStyles;
@use "@styles/partials/media-queries";
@use "@styles/partials/inline-dimens" as dimens;

:host {
  transition: ease all 0.5s;
}

expanded-tiles {
  // Styles for the expanded tile container
  .panel {
    background-color: #f0f0f0; // Example: change background
    border-radius: var(--expanded-tile-border-radius);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }

  .custom-expanded-header {
    padding: 15px;
    background-color: #333;
    color: white;
    h3 {
      margin-top: 0;
      margin-bottom: 5px;
    }
    p {
      margin-bottom: 0;
      font-size: 0.9em;
    }
  }

  // Styles for the content sections
  .panel-left, .panel-right {
    /* Add your specific styles here */
  }

  // Styles for the image/video wrapper
  .image-wrapper {
    /* Add your specific styles here */
  }

  // Styles for the right panel content
  .content-inner-wrapper {
    /* Add your specific styles here */
  }
}
````

## For Web Editor Users:

As a web editor user, you'll typically interact with the widget through the Nosto Admin Portal's "Custom Code" section. This means you'll be working with compiled HTML, CSS, and JavaScript.

1. Understanding the Expanded Tile When a user clicks on a tile, it can expand to show more details. This "expanded tile" is a special overlay that appears. To change what's inside it, you'll need to modify the underlying code that controls its appearance.
2. How to Modify the Expanded Tile Content You'll make your changes in four key boxes within the Custom Code editor:

Box 1: Custom JS

```javascript
// This script overrides the default expanded tile template
// It tells the widget to look for a custom template you define.

// Basic check if the sdk is available to prevent errors
sdk.addTemplateToComponent(function(sdk, component) {
    // This is where your custom HTML for the expanded tile will go.
    // We'll put the actual HTML in "Box 3: Widget Layout" for easier management.
    // For now, we'll return a basic structure.
    const tile = sdk.getTile(); // Get the data of the tile that was clicked [cite: 2028]

    if (!tile) {
        return ''; // If no tile data, return empty
    }

    // IMPORTANT: Replace 'ugc-widget-YOUR_WIDGET_ID_HERE' with your actual widget ID from the embed code.
    // You can find your widget ID in the widget embed code (data-id attribute).
    const widgetId = 'ugc-widget-122074'; // Example ID, replace with your actual widget ID [cite: 1399]

    return `
        <div class="expanded-tile-wrapper">
            <a class="exit" href="#">
                <span class="widget-icon close-white"></span>
            </a>
            <div class="panel">
                <div class="panel-left">
                    <img src="${tile.image}" alt="${tile.description || 'Tile Image'}" style="width: 100%; height: 100%; object-fit: cover;">
                </div>
                <div class="panel-right">
                    <div class="custom-expanded-header">
                        <h3>Details for: ${tile.name || 'Untitled Content'}</h3>
                        <p>Source: ${tile.source}</p>
                        <p>Date: <time-phrase source-created-at="${tile.source_created_at}"></time-phrase></p>
                    </div>
                    <div class="content-wrapper">
                        <div class="content-inner-wrapper">
                            <p>${tile.message || 'No description available.'}</p>
                            ${tile.tags_extended && tile.tags_extended.length > 0 ? `<tile-tags tile-id="${tile.id}" mode="swiper" context="expanded" navigation-arrows="true"></tile-tags>` : ''}
                            ${tile.tags_extended && sdk.getProductTagsFromTile(tile).length > 0 ? `<ugc-products parent="${widgetId}" tile-id="${tile.id}"></ugc-products>` : ''}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    `;
}, 'expanded-tiles');
```

Box 2: Custom CSS

```css
/* Styling for the expanded tile container */
.expanded-tile-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9); /* Dark overlay */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000; /* Ensure it's on top of other content */
}

/* Styles for the close button */
.expanded-tile-wrapper .exit {
  position: absolute;
  top: 20px;
  right: 20px;
  cursor: pointer;
  z-index: 1001;
}

.expanded-tile-wrapper .close-white {
  width: 30px;
  height: 30px;
  background-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>');
  background-repeat: no-repeat;
  background-size: contain;
}

/* Styles for the main panel containing the tile content */
.expanded-tile-wrapper .panel {
  display: flex;
  background-color: #ffffff; /* White background for the content area */
  border-radius: 8px;
  overflow: hidden;
  max-width: 90%; /* Adjust as needed */
  max-height: 90vh; /* Adjust as needed */
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
}

/* Styles for the left section (image/video) */
.expanded-tile-wrapper .panel-left {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}

/* Styles for the right section (text content) */
.expanded-tile-wrapper .panel-right {
  flex: 1;
  padding: 20px;
  overflow-y: auto; /* Enable scrolling for long content */
}

/* Custom Header Styling */
.expanded-tile-wrapper .custom-expanded-header {
    background-color: #f8f8f8;
    padding: 15px;
    border-bottom: 1px solid #eee;
    margin-bottom: 15px;
}

.expanded-tile-wrapper .custom-expanded-header h3 {
    margin: 0;
    color: #333;
    font-size: 1.2em;
}

.expanded-tile-wrapper .custom-expanded-header p {
    margin: 5px 0 0;
    color: #666;
    font-size: 0.9em;
}

/* Basic styling for other elements in the right panel if they exist */
.expanded-tile-wrapper .content-wrapper p {
  color: #333;
  line-height: 1.5;
}

/* Adjustments for smaller screens */
@media (max-width: 768px) {
  .expanded-tile-wrapper .panel {
    flex-direction: column; /* Stack sections vertically on small screens */
    max-width: 95%;
    max-height: 95vh;
  }
}
```

Congratulations! You have successfully modified the contents of an expanded tile in Nosto UGC NextGen widgets. You can further customize the HTML and CSS to fit your brand's needs.
