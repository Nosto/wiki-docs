You can query all the configured segments using the GraphQL Segments endpoint. You are able to query all the names and identifiers of the segments.

```graphql
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  segments{
    segments{
      id,
      name
    }
  }
}
EOF
```


### Sample Output

```json
{
  "data": {
    "segments": {
      "segments": [
        {
          "id": "5a497a000000000000000001",
          "name": "First-Time Visitors"
        },
        {
          "id": "5b71f1500000000000000006",
          "name": "Returning Visitors"
        },
        {
          "id": "5a497a000000000000000002",
          "name": "Prospects"
        },
        {
          "id": "5a497a000000000000000003",
          "name": "First-Time Customers"
        },
        {
          "id": "5a497a000000000000000004",
          "name": "Repeat Customers"
        },
        {
          "id": "5a497a000000000000000005",
          "name": "Loyal Customers"
        },
        {
          "id": "5a497a000000000000000000",
          "name": "All Customers"
        }
      ]
    }
  }
}
```