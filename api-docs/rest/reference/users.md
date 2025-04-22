# Users API

* [Properties](users.md#users-properties)
* [GET /api/users](users.md#GET-/api/users)
* [GET /api/users/:userId](users.md#GET-/api/users/userId)
* [PUT /api/users/:userId](users.md#PUT-/api/users/userId)
* [DELETE /api/users/:userId](users.md#DELETE-/api/users/userId)

## Users

Users are resources that have access to the Nosto admin portal. Users can't be created via API but are created by individuals following the invitation flow.

### Properties

| Field             | Type            | Value                                                                                     | PUT   | Definition                                                                    |
| ----------------- | --------------- | ----------------------------------------------------------------------------------------- | ----- | ----------------------------------------------------------------------------- |
| id                | integer         |                                                                                           | **X** | User Id                                                                       |
| username          | string          |                                                                                           | **X** | Username                                                                      |
| name              | string          |                                                                                           | ✔     | First name                                                                    |
| surname           | string          |                                                                                           | ✔     | Last name                                                                     |
| email             | string          |                                                                                           | ✔     | Email                                                                         |
| description       | string          |                                                                                           | ✔     | Notes on User                                                                 |
| display\_timezone | string          | [tz database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)               | ✔     | Timezone used when displaying date time strings                               |
| distances         | string          | <p><code>metric</code><br><code>imperial</code></p>                                       | ✔     | Distance unit system to use when handling geo location distances              |
| acl\_groups       | string or array | <p><code>Admin = 1</code><br><code>Moderator = 2</code><br><code>Developer = 3</code></p> | ✔     | A list of acl group ids, comma separated or JSON array when used in JSON body |
| last\_login       | datetime        |                                                                                           | **X** | User's last login datetime                                                    |
| created\_at       | timestamp       |                                                                                           | **X** | User's creation time                                                          |
| updated\_at       | timestamp       |                                                                                           | **X** | User's modification time                                                      |

[Back to Top](users.md#top)

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

[Back to Top](users.md#top)

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

[Back to Top](users.md#top)

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

[Back to Top](users.md#top)

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

[Back to Top](users.md#top)
