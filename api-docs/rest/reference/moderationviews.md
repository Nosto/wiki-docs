# Moderation Views API

* [Properties](moderationviews.md#moderationviews-properties)
* [GET /api/moderationviews](moderationviews.md#GET-/api/moderationviews)
* [POST /api/moderationviews](moderationviews.md#POST-/api/moderationviews)
* [GET /api/moderationviews/:moderationviewId](moderationviews.md#GET-/api/moderationviews/moderationviewId)
* [PUT /api/moderationviews/:moderationviewId](moderationviews.md#PUT-/api/terms/moderationviews)
* [DELETE /api/moderationviews/:moderationviewId](moderationviews.md#DELETE-/api/moderationviews/termId)

## Moderationviews

Moderationview allows users to save the view of a Moderation page, including content filter and the hide/show columns.

### Properties

| Field                                  | Type      | GET   | POST  | PUT   | Definition                                                                              |
| -------------------------------------- | --------- | ----- | ----- | ----- | --------------------------------------------------------------------------------------- |
| id                                     | integer   | ✔     | **X** | **X** | unique id for moderation view                                                           |
| view\_name                             | string    | ✔     | ✔     | ✔     | Moderationview's name                                                                   |
| type                                   | bool      | ✔     | ✔     | ✔     | `user`                                                                                  |
| access\_type                           | string    | ✔     | ✔     | ✔     | `public`                                                                                |
| stack\_id                              | integer   | ✔     | **X** | **X** |                                                                                         |
| owner\_id                              | integer   | ✔     | **X** | **X** |                                                                                         |
| column                                 | object    | ✔     | **X** | **X** | Moderationview's columns                                                                |
| columns\_select\_col                   | boolean   | **X** | ✔     | ✔     | Option to show 'select' column                                                          |
| columns\_pin\_col                      | boolean   | **X** | ✔     | ✔     | Option to show 'pin' column                                                             |
| columns\_scheduled\_col                | boolean   | **X** | ✔     | ✔     | Option to show 'scheduled' column                                                       |
| columns\_platform\_col                 | boolean   | **X** | ✔     | ✔     | Option to show 'platform' column                                                        |
| columns\_user\_col                     | boolean   | **X** | ✔     | ✔     | Option to show 'user' column                                                            |
| columns\_name\_col                     | boolean   | **X** | ✔     | ✔     | Option to show 'name' column                                                            |
| columns\_avatar\_col                   | boolean   | **X** | ✔     | ✔     | Option to show 'avatar' column                                                          |
| columns\_content\_col                  | boolean   | **X** | ✔     | ✔     | Option to show 'content' column                                                         |
| columns\_preview\_col                  | boolean   | **X** | ✔     | ✔     | Option to show 'preview' column                                                         |
| columns\_created\_col                  | boolean   | **X** | ✔     | ✔     | Option to show 'created' column                                                         |
| columns\_ingested\_col                 | boolean   | **X** | ✔     | ✔     | Option to show 'ingested' column                                                        |
| columns\_status\_col                   | boolean   | **X** | ✔     | ✔     | Option to show 'status' column                                                          |
| columns\_disabled\_reason\_col         | boolean   | **X** | ✔     | ✔     | Option to show 'disabled reason' column                                                 |
| columns\_last\_moderated\_col          | boolean   | **X** | ✔     | ✔     | Option to show 'last moderated' column                                                  |
| columns\_moderator\_col                | boolean   | **X** | ✔     | ✔     | Option to show 'moderator' column                                                       |
| columns\_terms\_col                    | boolean   | **X** | ✔     | ✔     | Option to show 'terms' column                                                           |
| columns\_tags\_col                     | boolean   | **X** | ✔     | ✔     | Option to show 'tags' column                                                            |
| columns\_type\_col                     | boolean   | **X** | ✔     | ✔     | Option to show 'type' column                                                            |
| columns\_star\_col                     | boolean   | **X** | ✔     | ✔     | Option to show 'star' column                                                            |
| columns\_abuse\_col                    | boolean   | **X** | ✔     | ✔     | Option to show 'abuse' column                                                           |
| columns\_no\_purge\_col                | boolean   | **X** | ✔     | ✔     | Option to show 'Purgeable' column                                                       |
| columns\_num\_votes                    | boolean   | **X** | ✔     | ✔     | Option to show 'num votes' column                                                       |
| columns\_last\_comment\_queued\_at     | boolean   | **X** | ✔     | ✔     | Option to show 'last comment queued at' column                                          |
| columns\_greatest\_score               | boolean   | **X** | ✔     | ✔     | Option to show 'greatest score' column                                                  |
| columns\_actions\_col                  | boolean   | **X** | ✔     | ✔     | Option to show 'actions' column                                                         |
| filter                                 | object    | ✔     | **X** | **X** | Filter criteria                                                                         |
| filter\_value\_content\_search         | string    | **X** | ✔     | ✔     | String                                                                                  |
| filter\_value\_origin                  | string    | **X** | ✔     | ✔     | Network types in CSV                                                                    |
| filter\_value\_media                   | string    | **X** | ✔     | ✔     | Media types in CSV                                                                      |
| filter\_value\_abuse                   | boolean   | **X** | ✔     | ✔     |                                                                                         |
| filter\_value\_filter\_by\_claimed     | string    | **X** | ✔     | ✔     | Filter criteria `claimed_only`                                                          |
| filter\_value\_filter\_by\_geolocation | boolean   | **X** | ✔     | ✔     |                                                                                         |
| filter\_value\_tags                    | string    | **X** | ✔     | ✔     | tag ids in CSV                                                                          |
| filter\_value\_terms                   | string    | **X** | ✔     | ✔     | term ids in CSV                                                                         |
| other                                  | object    | ✔     | **X** | **X** | Additional config                                                                       |
| other\_all\_content\_length            | integer   | **X** | ✔     | ✔     | Number of items per page                                                                |
| other\_live\_updates                   | boolean   | **X** | ✔     | ✔     |                                                                                         |
| other\_display                         | string    | **X** | ✔     | ✔     | `table`                                                                                 |
| date                                   | object    | ✔     | **X** | **X** | Date range filter                                                                       |
| date\_range\_from                      | string    | **X** | ✔     | ✔     | ISO Date Time string                                                                    |
| date\_range\_to                        | string    | **X** | ✔     | ✔     | ISO Date Time string                                                                    |
| sort\_by                               | string    | ✔     | ✔     | ✔     | This is an option for sorting. The value will need to be one of the following: `newest` |
| created\_at                            | timestamp | ✔     | **X** | **X** | Moderationview's creation time                                                          |
| updated\_at                            | timestamp | ✔     | **X** | **X** | Moderationview's modification time                                                      |

[Back to Top](moderationviews.md#top)

### GET moderationviews

Retrieves all modertion views available in the Stack.

#### Resource URL

`https://api.stackla.com/api/moderationviews`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

No additional request parameters are available.

[Back to Top](moderationviews.md#top)

### POST moderationviews

Creates a new moderation view in the Stack.

#### Resource URL

`https://api.stackla.com/api/moderationviews`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

No additional request parameters are available.

[Back to Top](moderationviews.md#top)

### GET moderationviews/:moderationviewId

Retrieves a specific moderation view available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/moderationviews/:moderationviewId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name             | Mandatory | Request type | Description               |
| ---------------- | --------- | ------------ | ------------------------- |
| moderationviewId | Yes       | endpoint     | ID of the Moderation view |

[Back to Top](moderationviews.md#top)

### PUT moderationviews/:moderationviewId

Updates a specific moderation view available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/moderationviews/:moderationviewId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name             | Mandatory | Request type | Description               |
| ---------------- | --------- | ------------ | ------------------------- |
| moderationviewId | Yes       | endpoint     | ID of the Moderation view |

[Back to Top](moderationviews.md#top)

### DELETE moderationviews/:moderationviewId

Deletes a specific moderation view available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/moderationviews/:moderationviewId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name             | Mandatory | Request type | Description               |
| ---------------- | --------- | ------------ | ------------------------- |
| moderationviewId | Yes       | endpoint     | ID of the Moderation view |

[Back to Top](moderationviews.md#top)

## Users

Users are resources that have access to the Nosto admin portal. Users can't get created via API, but instead are created by individuals following the invitation flow.

### Properties

User Id   Username   First name   Last name   Email   Notes on User   User's last login datetime   User's creation time   User's modification time

| Field             | Type            | Value                                                                       | Definition                                                                    |
| ----------------- | --------------- | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| id                | integer         |                                                                             |                                                                               |
| username          | string          |                                                                             |                                                                               |
| name              | string          |                                                                             |                                                                               |
| surname           | string          |                                                                             |                                                                               |
| email             | string          |                                                                             |                                                                               |
| description       | string          |                                                                             |                                                                               |
| display\_timezone | string          | [tz database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) | Timezone used when displaying date time strings                               |
| distances         | string          | `metric` `imperial`                                                         | Distance unit system to use when handling geo location distances              |
| acl\_groups       | string or array | `Admin = 1` `Moderator = 2` `Developer = 3`                                 | A list of acl group ids, comma separated or JSON array when used in JSON body |
| last\_login       | datetime        |                                                                             |                                                                               |
| created\_at       | timestamp       |                                                                             |                                                                               |
| updated\_at       | timestamp       |                                                                             |                                                                               |

[Back to Top](moderationviews.md#top)

### GET users

Retrieves all users available in the Stack.

#### Resource URL

`https://api.stackla.com/api/users`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name                 | Mandatory | Request type | Description                                                                                                  |
| -------------------- | --------- | ------------ | ------------------------------------------------------------------------------------------------------------ |
| page                 | No        | query        | Page number. Default value is 1                                                                              |
| limit                | No        | query        | Return limit define how many Users will be return for each request. Default is 25. Maximum limit is 100.     |
| order\_by            | No        | query        | Return Users ordered by either `id, name, email, surname, username, created`. Sorts by `id` by default.      |
| order\_by\_direction | No        | query        | Return ordered list of Users in either ascending (`asc`) or descending (`desc`) direction. Default is `asc`. |

[Back to Top](moderationviews.md#top)

### GET users/:userId

Retrieves a specific user available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/users/:userId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name   | Mandatory | Request type | Description    |
| ------ | --------- | ------------ | -------------- |
| userId | Yes       | Request type | ID of the User |

[Back to Top](moderationviews.md#top)

### PUT users/:userId

Updates a specific user available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/users/:userId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name   | Mandatory | Request type | Description    |
| ------ | --------- | ------------ | -------------- |
| userId | Yes       | endpoint     | ID of the User |

[Back to Top](moderationviews.md#top)

### DELETE users/:userId

Deletes a specific user available in the Stack by its ID.

#### Resource URL

`https://api.stackla.com/api/users/:userId`

#### Resource Details

Rate limited: Yes

Access scope: User

#### Request Parameters

| Name   | Mandatory | Request type | Description    |
| ------ | --------- | ------------ | -------------- |
| userId | Yes       | endpoint     | ID of the User |

[Back to Top](moderationviews.md#top)
