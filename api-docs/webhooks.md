# Webhooks

## Overview

Nosto's UGC Webhooks enable callbacks to be made to your HTTP(S) endpoints when specific events have been triggered (often referred to as _notifications_).

For each of the Webhook types you can enter the URL of your endpoints, and Nosto's UGC will make HTTP POST requests with the relevant data to the endpoints when the events occur. The URLs entered can be different, or the same URL can be configured for all of them and the payload of the POST can be used to determine the Webhook.

To start receiving notifications, simply enter the relevant URLs in the _Stackla API_ > _Webhooks_ area of [your Stack's Admin Portal](https://my.stackla.com). To stop receiving notifications, simply remove the URL from the Webhook's field.

Webhook notifications are generally in near-realtime, however during times of very high load, there may be a delay of several seconds before the notification arrives.

## Security Restrictions

Webhooks may originate from a variety of servers and IP addresses, by default. If you require to whitelist IP address ranges to receive webhooks, please contact our support team or your Customer Success Manager to organize a custom solution.

Nosto's UGC Webhooks also support appending a 'Secret Key' to the Notification if required.

This secret key uses X-Stackla-Webhook-Signature header to sign the request with a secret key.&#x20;

This secret key can be used to validate the request from the client's webhook endpoint.&#x20;

We do not support ip ranges, but if a client requests this, they should use ap-southeast-2 ip ranges. https://ip-ranges.amazonaws.com/ip-ranges.json

## Notification Data

Notifications will arrive as HTTP POST requests, with Content-Type set to "application/json". This may mean a slightly different strategy for grabbing POST data for languages such as PHP, which will not be able to read directly from $\_POST.



## Webhook secret key

The secret key is sent in the HTTP header named `X-Stackla-Webhook-Signature` , and the value is crypto-encrypted

```
const signature = crypto.createHmac('sha256', secret)
            .update(data)
            .digest('hex');
```

## Webhook Types

Nosto's UGC supports the following types of Webhooks:

* [TILE\_CLAIM](webhooks.md#webhook-tile-claim)
* [TILE\_STATUS\_UPDATE](webhooks.md#webhook-tile-status-update)
* [TILE\_TAGS\_UPDATE](webhooks.md#webhook-tile-tags-update)
* [TILE\_VOTES\_UPDATE](webhooks.md#webhook-tile-votes-update)
* [TILE\_INGESTED](webhooks.md#webhook-tile-ingested)
* [TILE\_LIKE\_DISLIKE](webhooks.md#webhook-tile-like-dislike)
* [USER\_UPDATE](webhooks.md#webhook-user-update)
* [ASSET\_CREATED](webhooks.md#webhook-asset-created)
* [ASSET\_UPDATED](webhooks.md#webhook-asset-updated)
* [MANUAL\_TILE\_WEBHOOK](webhooks.md#webhook-manual-tile)
* [MANUAL\_ASSET\_WEBHOOK](webhooks.md#webhook-manual-asset)

#### TILE\_CLAIM

A callback will be made via HTTP POST when an individual Tile has been claimed.

**Data**

| Field                             | Description                                                         |
| --------------------------------- | ------------------------------------------------------------------- |
| \_id                              | Nosto's UGC ID of the Tile                                          |
| sta\_feed\_id                     | External ID provided by the UGC Feed or API posted Tile             |
| guid                              | Alias of _sta\_feed\_id_                                            |
| network                           | Name of network user belongs to (e.g. "twitter", "instagram", etc.) |
| network\_id                       | ID of original content on the network                               |
| network\_user\_id                 | ID of user on the network                                           |
| network\_user\_name               | Display name of user on the network                                 |
| network\_user                     | Handle of user on the network                                       |
| action                            | "TILE\_CLAIM"                                                       |
| votes                             | Number of votes currently on the Tile                               |
| claimed\_data.claimed\_at         | Timestamp of the claim action                                       |
| claimed\_data.claimed\_by         | Claiming method: "hashtag" (by response)                            |
| claimed\_data.message             | If claim is actioned by response, this will be the response message |
| claimed\_data.custom\_data.\[key] | Value of the custom field `key`                                     |

**Sample**

```
{"_id":"5e27c2c529a27762badd642b","action":"TILE_CLAIM","sta_feed_id":"690549978851fd782e1ebc2c413f974d","guid":"690549978851fd782e1ebc2c413f974d","network":"social_network","network_id":"690549978851fd782e1ebc2c413f974d","network_user":null,"network_user_id":"550b99e2aa7ba","network_user_name":"Content Creator","original_post_url":"https://www.website.url","original_user_url":"https://www.website.url","stack_id":1234,"claimed_data":{"claimed_at":"1579664135","claimed_by":"Content Creator","message":"Rights Granted"},"updated_time":1579664135}
```

[Back to top](webhooks.md#section-overview)

#### TILE\_STATUS\_UPDATE

A callback will be made via HTTP POST when an individual Tile's status has been updated.

**Data**

| Field                 | Description                                                                     |
| --------------------- | ------------------------------------------------------------------------------- |
| \_id                  | Nosto's UGC ID of the Tile                                                      |
| sta\_feed\_id         | External ID provided by the UGC Feed or API posted Tile                         |
| guid                  | Alias of _sta\_feed\_id_                                                        |
| network               | Name of network the post/tile originated on (e.g. "twitter", "instagram", etc.) |
| network\_id           | ID of post on the network                                                       |
| action                | "TILE\_STATUS\_UPDATE"                                                          |
| status                | The new status of the tile: "published"                                         |
| old\_status           | The previous status of the tile: "published"                                    |
| terms                 | An array of the Term IDs that the tile is associated with                       |
| tags                  | An array of the Tag IDs that have been assigned for the Tile                    |
| updated\_time         | Timestamp of event generation                                                   |
| custom\_fields.\[key] | Value of the custom field `key`                                                 |

**Sample**

```
{"_id":"5e27c2c529a27762badd642b","action":"TILE_STATUS_UPDATE","sta_feed_id":"690549978851fd782e1ebc2c413f974d","guid":"690549978851fd782e1ebc2c413f974d","network":"social_network","network_id":"690549978851fd782e1ebc2c413f974d","network_user":null,"network_user_id":"550b99e2aa7ba","network_user_name":"Content Creator","custom_fields":[],"original_post_url":"","original_user_url":"https://www.website.url","stack_id":1234,"status":"queued","terms":[12345],"tags":["12345","23456","34567"],"sentiment_score":0,"external_data":{"firstname":"Content","lastname":"Creator","email":"email@address.com", "terms_and_conditions":1,"_id":{"$oid":"5e27c2c4a15d4162ba5d8a7e"}},"old_status":"published","updated_time":1579664379}
```

**Usage Examples**

* [Trigger notifications when content is in the moderation queue](../guides/webhooks/trigger-notifications-when-content-in-queue.md)

[Back to top](webhooks.md#section-overview)

#### TILE\_TAGS\_UPDATE

A callback will be made via HTTP POST when an individual Tile has had its tags updated.

**Data**

| Field         | Description                                                                     |
| ------------- | ------------------------------------------------------------------------------- |
| \_id          | Nosto's UGC ID of the Tile                                                      |
| sta\_feed\_id | External ID provided by the UGC Feed or API posted Tile                         |
| guid          | Alias of _sta\_feed\_id_                                                        |
| network       | Name of network the post/tile originated on (e.g. "twitter", "instagram", etc.) |
| network\_id   | ID of post on the network                                                       |
| action        | "TILE\_TAGS\_UPDATE"                                                            |
| tags          | An array of the Tag IDs                                                         |
| old\_tags     | An array of the previous Tag IDs                                                |
| updated\_time | Timestamp of event generation                                                   |

**Sample**

```
{"_id":"5e27c2c529a27762badd642b","action":"TILE_TAGS_UPDATE","sta_feed_id":"690549978851fd782e1ebc2c413f974d","guid":"690549978851fd782e1ebc2c413f974d","network":"social_network","network_id":"690549978851fd782e1ebc2c413f974d","network_user":"Content Creator","network_user_id":"550b99e2aa7ba","network_user_name":"Content Creator","custom_fields":[],"original_post_url":"","original_user_url":"","stack_id":1234,"tags":["12345","23456","34567"],"old_tags":["12345","34567"],"updated_time":1579664376}
```

[Back to top](webhooks.md#section-overview)

#### TILE\_VOTES\_UPDATE

A callback will be made via HTTP POST when an individual Tile has been voted on.

**Data**

| Field         | Description                                                                     |
| ------------- | ------------------------------------------------------------------------------- |
| \_id          | Nosto's UGC ID of the Tile                                                      |
| sta\_feed\_id | External ID provided by the UGC Feed or API posted Tile                         |
| guid          | Alias of _sta\_feed\_id_                                                        |
| network       | Name of network the post/Tile originated on (e.g. "twitter", "instagram", etc.) |
| network\_id   | ID of post on the network                                                       |
| action        | "TILE\_VOTES\_UPDATE"                                                           |
| votes         | Number of current votes for the Tile                                            |

[Back to top](webhooks.md#section-overview)

#### TILE\_INGESTED

A callback will be made via HTTP POST when an individual tile has been ingested.

**Data**

| Field                 | Description                                                                     |
| --------------------- | ------------------------------------------------------------------------------- |
| \_id                  | Nosto's UGC ID of the Tile                                                      |
| sta\_feed\_id         | External ID provided by the UGC Feed or API posted Tile                         |
| guid                  | Alias of _sta\_feed\_id_                                                        |
| network               | Name of network the post/Tile originated on (e.g. "twitter", "instagram", etc.) |
| network\_id           | ID of post on the network                                                       |
| action                | "TILE\_INGESTED"                                                                |
| status                | The new status of the Tile: "published"                                         |
| terms                 | An array of the Term IDs that the tile is associated with                       |
| tags                  | An array of the Tag IDs that have been assigned for the Tile                    |
| updated\_time         | Timestamp of event generation                                                   |
| sentiment\_score      | Numerical representation of the sentiment polarity                              |
| custom\_fields.\[key] | Value of the custom field `key`                                                 |

**Sample**

```
{"_id":"5e27c2c529a27762badd642b","action":"TILE_INGESTED","sta_feed_id":"690549978851fd782e1ebc2c413f974d","guid":"690549978851fd782e1ebc2c413f974d","network":"sta_feed","network_id":"690549978851fd782e1ebc2c413f974d","network_user":null,"network_user_id":"550b99e2aa7ba","network_user_name":"First Name Last Name","original_post_url":"","original_user_url":"","stack_id":1277,"status":"published","terms":[49349],"tags":["111635","146566"],"sentiment_score":0,"external_data":{"firstname":"First Name","lastname":"Last Name","email":"email@address.com","ip":"59.100.203.78","terms_and_conditions_url":"http://www.stackla.com","terms_and_conditions":1},"created_time":1579664069}
```

[Back to top](webhooks.md#section-overview)

#### TILE\_LIKE\_DISLIKE

A callback will be made via HTTP POST when an individual tile has been like or dislike.

**Data**

| Field         | Description                                                                     |
| ------------- | ------------------------------------------------------------------------------- |
| \_id          | Nosto's UGC ID of the Tile                                                      |
| sta\_feed\_id | External ID provided by the UGC Feed or API posted Tile                         |
| guid          | Alias of _sta\_feed\_id_                                                        |
| network       | Name of network the post/Tile originated on (e.g. "twitter", "instagram", etc.) |
| network\_id   | ID of post on the network                                                       |
| action        | "TILE\_LIKE\_DISLIKE"                                                           |
| likes         | Number of current like for the Tile                                             |
| dislikes      | Number of current dislike for the Tile                                          |
| type          | The action type: "like"                                                         |
| updated\_time | Timestamp of event generation                                                   |

**Usage Examples**

* [Trigger notifications when content is in the moderation queue](../guides/webhooks/trigger-notifications-when-content-in-queue.md)

[Back to top](webhooks.md#section-overview)

#### USER\_UPDATE

A callback will be made via HTTP POST when a new user is successfully registered or an existing user profile is updated.

**Data**

| Field             | Description                   |
| ----------------- | ----------------------------- |
| id                | Nosto's UGC ID of the User    |
| name              | User First Name               |
| surname           | User Last Name                |
| type              | The action type: "added"      |
| username          | Username                      |
| action            | "USER\_UPDATE"                |
| display\_timezone | User Timezone                 |
| email             | User Email Address            |
| created           | Timestamp of event generation |

[Back to top](webhooks.md#section-overview)

#### ASSET\_CREATED

A callback will be made via HTTP POST when

**Data**

**Sample**

```
{"workflow_status":"approved","from":"goconnect","created_at":1579664073,"title":"Content Creator - GoConnect - 22nd Jan 2020 03:34 UTC","note":"","tags":["12345","23456"],"tile":{"_id":"5e27c2c529a27762badd642b","source":"social_network","source_created_at":1579664065,"source_unique_id":"sta_feed:690549978851fd782e1ebc2c413f974d","source_user_id":"550b99e2aa7ba","user":null,"name":"Content Creator","original_url":"https://www.website.url","claimed":true,"message":"Sample Message"},"type":"image","thumbnail":{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-small.jpg","mime":"image/jpeg","size":31438,"width":640,"height":359},"media":{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","mime":"image/jpeg","size":102029,"width":1280,"height":719,"orientation":"landscape","dimension":"m"},"media_files":[{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","mime":"image/jpeg","size":102029,"width":1280,"height":719},{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-large.jpg","mime":"image/jpeg","size":226643,"width":1824,"height":1024},{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-small.jpg","mime":"image/jpeg","size":31438,"width":640,"height":359}],"_id":"5e27c2c875986fa79afb6621"}
```

[Back to top](webhooks.md#section-overview)

#### ASSET\_UPDATED

A callback will be made via HTTP POST when

**Data**

**Sample**

```
{"workflow_status":"approved","from":"goconnect","created_at":1579664073,"title":"Content Creator - GoConnect - 22nd Jan 2020 03:34 UTC","note":"","tags":["12345","23456"],"tile":{"_id":"5e27c2c529a27762badd642b","source":"social_network","source_created_at":1579664065,"source_unique_id":"sta_feed:690549978851fd782e1ebc2c413f974d","source_user_id":"550b99e2aa7ba","user":null,"name":"Content Creator","original_url":"https://www.website.url","claimed":true,"message":"Sample Message"},"type":"image","thumbnail":{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-small.jpg","mime":"image/jpeg","size":31438,"width":640,"height":359},"media":{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","mime":"image/jpeg","size":102029,"width":1280,"height":719,"orientation":"landscape","dimension":"m"},"media_files":[{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","mime":"image/jpeg","size":102029,"width":1280,"height":719},{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-large.jpg","mime":"image/jpeg","size":226643,"width":1824,"height":1024},{"url":"https://uploads-cdn.stackla.com/10/stack/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-small.jpg","mime":"image/jpeg","size":31438,"width":640,"height":359}],"_id":"5e27c2c875986fa79afb6621" "updated_at":1579664845,"oldValues":{"workflow_status":"approved","title":"Previous Title - GoConnect - 22nd Jan 2020 03:34 UTC","note":"","tags":["111635","146566"],"updated_at":1579664844}}
```

[Back to top](webhooks.md#section-overview)

#### MANUAL\_TILE\_UPDATE

A callback will be made via HTTP POST when

**Data**

**Sample**

```
{"_id":"5e27c2c529a27762badd642b","avatar":null,"claimed":true,"claimed_by":"goconnect","created_at":1579664069,"image":"https://uploads-cdn.stackla.com/10/stackla/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","lang":"en","media":"image","message":"Message","tags":["12345","23456","34567"],"updated_at":1579664381,"claimed_data":{"claimed_at":"2020-01-22T03:34:25.000Z","claimed_by":"goconnect","message":"Message"},"network_user_name":"Content Creator","original_post_url":"https://www.website.url","network":"social_network","network_user_id":"550b99e2aa7ba"}
```

#### MANUAL\_ASSET\_UPDATE

A callback will be made via HTTP POST when

**Data**

**Sample**

```
{"_id":"5e27c2c875986fa79afb6621","from":"goconnect","title":"Content Creator - GoConnect - 22nd Jan 2020 03:34 UTC","note":"","tags":[12345,23456],"type":"image","media_files":[{"url":"https://uploads-cdn.stackla.com/10/stackla/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","mime":"image/jpeg","size":102029,"width":1280,"height":719},{"url":"https://uploads-cdn.stackla.com/10/stackla/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-large.jpg","mime":"image/jpeg","size":226643,"width":1824,"height":1024},{"url":"https://uploads-cdn.stackla.com/10/stackla/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-small.jpg","mime":"image/jpeg","size":31438,"width":640,"height":359}],"created_at":1579664073,"thumbnail":{"url":"https://uploads-cdn.stackla.com/10/stackla/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-small.jpg","mime":"image/jpeg","size":31438,"width":640,"height":359},"media":{"url":"https://uploads-cdn.stackla.com/10/stackla/2020-01-22/5e27c2b17bd4e/42dd99b4ac68a4f5cbfadf2fb19dafa3-medium.jpg","mime":"image/jpeg","size":102029,"width":1280,"height":719}}
```
