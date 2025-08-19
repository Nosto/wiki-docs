# Composition of tiles

**Tile**

A **Tile** is the central entity in Visual UGC, representing a curated piece of user-generated content. Tiles are enhanced with metadata, media, tags, and interactivity, enabling seamless integration into marketing campaigns or other content strategies.

* **Core Attributes**:
  * `preloaded` (boolean, optional): Indicates if the Tile is preloaded in Visual UGC.
  * `_id` (object, optional): MongoDB-style object containing `$id` (string).
  * `carousel` (number): Position of the Tile in a Stackla carousel.
  * `created_at` (number): Timestamp when the Tile was added to Stackla.
  * `disabled` (boolean): Indicates if the Tile is disabled from public display.
  * `hotspots` (array of Hotspots, optional): Interactive regions within the Tile for tagging products or adding links.
  * `image` (string): URL of the primary image in the Tile.
  * `avatar` (string): URL of the avatar image associated with the original user/content creator.
  * `image_expire_at` (number): Expiration timestamp for the Tile's primary image.
  * `lang_detection_type` (string, optional): Detected language of the content within the Tile.
  * `media` (string): Media type for the Tile (e.g., image, video).
  * `message` (string): Caption or text description associated with the Tile.
  * `name` (string | null): Name or title of the Tile (often based on the source).
  * `original_url` (string): Original media source URL (e.g., link to an Instagram or Twitter post).
  * `original_link` (string): Link back to the original content on the platform it was pulled from.
  * `page_post` (boolean): Indicates if the Tile originated from a page post.
  * `reviewed` (boolean): Marks whether the Tile has been reviewed for moderation or tagging.
  * `score` (number): Sentiment or engagement score of the Tile.
  * `source` (string): The content's originating platform (e.g., Instagram, Twitter).
  * `tags` (array of strings): Tags applied to the Tile for categorization or marketing.
  * `tags_extended` (array of TagExtended, optional): Enhanced metadata for each tag.
  * `terms` (array of strings): Terms or keywords associated with the Tile.
  * `video` (Video, optional): Metadata about video content for the Tile.
  * `video_files` (array of VideoFile, optional): Video file details for playback or export.
  * `original_image_url` (string): URL of the unaltered original image from the source platform.
  * `embed_url` (string): Embed URL for the Tile, allowing integration with external sites.
  * `youtube_id` (string): YouTube video ID, if the Tile references a YouTube video.
  * `title` (string): Title or heading for the Tile.
  * `full_embed_html` (string): Full HTML snippet for embedding the Tile into external content.
  * `attrs` (array of strings): Additional attributes or metadata for the Tile.

***

**Hotspot**

A **Hotspot** is an interactive region within a Tile, designed for tagging products or adding links. These are often used to highlight shoppable elements within UGC.

* **Attributes**:
  * `id` (number): Unique identifier for the Hotspot.
  * `x` (number): X-coordinate for the Hotspot's position within the Tile.
  * `y` (number): Y-coordinate for the Hotspot's position within the Tile.
  * `width` (number): Width of the interactive area.
  * `height` (number): Height of the interactive area.
  * `type` (string): Type of Hotspot (e.g., product link, custom link).
  * `custom_url` (string): Custom URL assigned to the Hotspot for redirection.
  * `product_link_attribute` (string): Product-specific attributes for shoppable Hotspots.
  * `tag` (TagExtended): Associated metadata tag for the Hotspot.
  * `coords` (array of numbers): Coordinates defining the Hotspot's position and size.

***

**TagExtended**

The **TagExtended** type represents a comprehensive tagging system that combines basic tagging functionality with additional metadata for products, categories, and campaigns. These tags are often used to link user-generated content with specific marketing initiatives.

* **Attributes**:
  * `id` (string): Unique identifier for the tag.
  * `active` (boolean, optional): Indicates whether the tag is currently active.
  * `auto_apply` (boolean, optional): Indicates if the tag is automatically applied based on certain criteria.
  * `availability` (number): Availability status of the tagged item.
  * `availability_status` (string, optional): Additional information about item availability.
  * `brand` (string, optional): Brand associated with the tag.
  * `categories` (string, optional): Categories linked to the tag.
  * `created_at` (string, optional): Timestamp when the tag was created.
  * `cta_text` (string, optional): Call-to-action text linked to the tag.
  * `currency` (string, optional): Currency format for the price.
  * `custom_fields` (string, optional): Custom metadata fields for the tag.
  * `custom_slug` (string, optional): Custom slug for the tag.
  * `custom_url` (string): URL linked to the tag.
  * `description` (string, optional): Description of the tagged item.
  * `ext_product_id` (string, optional): External product ID associated with the tag.
  * `image` (string, optional): URL of the image representing the tag.
  * `is_cross_seller` (boolean): Indicates if the tag supports cross-selling.
  * `price` (string): Price associated with the tag.
  * `type` (string): Type of the tag (e.g., product, campaign).
  * `variant` (array of Variant, optional): List of product variants linked to the tag.

***

**Variant**

A **Variant** represents a specific version of a product, allowing multiple configurations to be associated with a single tag.

* **Attributes**:
  * `id` (string): Unique identifier for the variant.
  * `availability` (string): Availability status of the product variant.
  * `brand` (string): Brand name associated with the variant.
  * `condition` (string): Product condition (e.g., new, used).
  * `description` (string): Description of the product variant.
  * `google_product_category` (string): Google-defined product category.
  * `group` (string): Identifier for product grouping.
  * `image_link` (string): URL of the product variant's image.
  * `item_group_id` (string): Group ID for related variants.
  * `link` (string): Product page URL for the variant.
  * `price` (string): Price of the product variant.
  * `title` (string): Title of the product variant.
  * `via_feed` (number | null): Indicates whether the variant was imported via a product feed.

***

**Video**

A **Video** object contains details about standard-resolution video content for a Tile.

* **Attributes**:
  * `standard_resolution` (object):
    * `url` (string): URL of the video in standard resolution.

***

**VideoFile**

The **VideoFile** type provides metadata for individual video files, enabling playback or download.

* **Attributes**:
  * `url` (string): URL of the video file.
  * `mime` (string): MIME type of the video file (e.g., `video/mp4`).
  * `size` (number): Size of the file in bytes.
  * `width` (number): Video width in pixels.
  * `height` (number): Video height in pixels.
  * `duration` (number): Duration of the video in seconds.
