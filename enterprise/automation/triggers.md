# Triggers

## Overview

Triggers define the respective condition(s) upon which an Automation Rule will be run against a Tile or Asset that has been aggregated into your respective Stack.

## Operations

Nosto's UGC supports a range of comparison, logical, element, elevation, and array query operators, allowing clients to define complex triggers to best define their Automation Rules.

These operations can be utilized when defining Rules via the [**Rules API**](https://github.com/Stackla/docs/blob/master/automation/api/README.md)

### Comparison Query Operators

| Operator | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| $gte     | Matches values that are greater than or equal to a specified value. |
| $gt      | Matches values that are greater than a specified value.             |
| $lte     | Matches values that are less than or equal to a specified value.    |
| $lt      | Matches values that are less than a specified value.                |
| $eq      | Matches values that are equal to a specified value.                 |
| $ne      | Matches all values that are not equal to a specified value.         |

### Logical Query Operators

| Operator | Description                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------- |
| $and     | Joins query clauses with a logical AND returns all documents that match the conditions of both clauses. |
| $or      | Joins query clauses with a logical OR returns all documents that match the conditions of either clause. |
| $nor     | Joins query clauses with a logical NOR returns all documents that fail to match both clauses.           |
| $not     | Inverts the effect of a query expression and returns documents that do not match the query expression.  |

### Element Query Operators

| Operator | Description                                            |
| -------- | ------------------------------------------------------ |
| $exists  | Matches documents that have the specified field.       |
| $type    | Selects documents if a field is of the specified type. |

### Elevation Query Operators

| Operator | Description                                                          |
| -------- | -------------------------------------------------------------------- |
| $regex   | Selects documents where values match a specified regular expression. |
| $where   | Matches documents that satisfy a JavaScript expression.              |

### Array Query Operators

| Operator   | Description                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------ |
| $all       | Matches arrays that contain all elements specified in the query.                                 |
| $elemMatch | Selects documents if element in the array field matches all the specified $elemMatch conditions. |
| $size      | Selects documents if the array field is a specified size.                                        |

## Fields

Automation Rules supports any data field available on a Tile or Asset object being used in a defined condition within the Trigger.

Listed below are some of the key parameters which could be considered when defining an Automation Rule via the [**Rules API**](https://github.com/Stackla/docs/blob/master/automation/api/README.md).

### Tile

| Field                     | Description                                           |
| ------------------------- | ----------------------------------------------------- |
| geohash                   | Location expressed in a short alphanumeric string     |
| lang                      | Identified Language on Tile                           |
| image\_tags               | Visual Recognition Concepts                           |
| location.name             | Name of Location associated with Tile                 |
| media                     | Media Type of the Tile                                |
| message                   | Text Message associated with Tile                     |
| name                      | Username associated with Tile                         |
| user                      | User Handle associated with Tile                      |
| stackla\_sentiment\_score | Sentiment Score applied to the Tile                   |
| tags                      | Tag(s) applied to the Tile                            |
| terms                     | Term(s) in which the Tile has been aggregated through |

### Asset

| Field          | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| claimed        | The Rights Status of the Asset                               |
| exif.make      | EXIF Make Information                                        |
| exif.model     | EXIF Model Information                                       |
| exif.software  | EXIF Software Information                                    |
| exif.copyright | EXIF Copyright Information                                   |
| image\_tags    | Visual Recognition Concepts                                  |
| media          | Media Type of the Tile                                       |
| message        | Text Message associated with Tile the Asset was created from |
| name           | Username associated with Tile the Asset was created from     |
| user           | User Handle associated with Tile the Asset was created from  |
| tags           | Tag(s) applied to the Tile                                   |

[Back to Top](triggers.md#top)
