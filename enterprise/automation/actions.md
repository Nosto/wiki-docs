# Actions

## Overview

Actions define what transformation(s) will be applied to a Tile or an Asset-based upon the defined conditions specified in the Trigger being met.

## Operations

Nosto's UGC Automation Rules support two operations when defining an Action. These operations define whether the transformation value is appended to an existing array or if it overwrites the particular field.

These operations can be utilized when defining Rules via the [**Rules API**](https://github.com/Stackla/docs/blob/master/automation/api/README.md)

| Operator  | Description                            |
| --------- | -------------------------------------- |
| $addToSet | Add element to the Array.              |
| $set      | Overwrite the select field with Value. |

## Fields

Automation Rules currently only support transforming a limited number of fields on both Tiles and Assets. Note: Not all fields can support all available operations, please consider this when building your Automation Rules via the [**Rules API**](https://github.com/Stackla/docs/blob/master/automation/api/README.md).

### Tiles

| Field                     | Operations      | Description                                                 |
| ------------------------- | --------------- | ----------------------------------------------------------- |
| tags                      | $set, $addToSet | Add a Tag or overwrite all Tags fo a Tile                   |
| location                  | $set            | Add a Location Name, Latitude and Longitude value to a Tile |
| stackla\_sentiment\_score | $set            | Set the Sentiment Score on a Tile                           |
| status                    | $set            | Set the Status of the Tile                                  |
| hotspot                   | $set            | Apply a Shopspot to the Image Asset on a Tile               |

### Assets

| Field            | Operations      | Description                                  |
| ---------------- | --------------- | -------------------------------------------- |
| tags             | $set, $addToSet | Add a Tag or overwrite all Tags for an Asset |
| collections      | $addToSet       | Add to a Collection                          |
| workflow\_status | $set            | Set the Workflow Status of an Asset          |
| right\_status    | $set            | Set the Rights Status for an Asset           |
| expired\_at      | $set            | Set the expiry date for an Asset             |

[Back to Top](actions.md#top)
