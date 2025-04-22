# Reference

## API Specification

This area details each of the available REST resources, their properties, and their methods. For general information and overview of the API such as authentication and pagination, refer to the [REST API overview documentation](https://github.com/Stackla/docs/blob/master/docs/api-docs/rest/README.md).

### Filters

A Filter is a query for Nosto's UGC Tile content that was ingested from a particular Network, contains a particular Tag, and/or is of a particular Medium. To retrieve a stream of content you can do so using the Filters endpoint.

[Filters API](filters.md)

### Moderationviews

Moderationview allows users to save the view of a Moderation page, including the content filter and the hide/show columns.

[Moderation views API](moderationviews.md)

### Tags

Tags allow content to be categorized and filtered for better curation. They are more like blog tags or product swing tags and are not to be confused with hashtags. It is a form of profiling Social Tiles for grouping/association purposes.

[Tags API](tags.md)

### Terms

Terms in Nosto's UGC allow us to set the rules to ingest content from different networks (such as user-generated content from Social Networks) - it is defined as an ingestion query within the Nosto platform.

[Terms API](terms.md)

### Tiles

The Tile is defined as any post/comment stored within Nosto's UGC that has been ingested from an external source (ie. Tweet from Twitter, Post from FB)

[Tiles API](tiles.md)

### Users

Users are resources that have access to the Nosto Admin Portal but. Users can't be created via API but are created by individuals following the invitation flow.

[Users API](users.md)

### Widgets

Widgets refer to any Nosto's UGC Social Visual that can be embedded onto a digital property that is made up of either an "In-Line" or possibly also an "Expanded Tile" view. This endpoint allows you to create widgets and returns an embed code.

[Widgets API](widgets.md)
