# Querying Segments and Affinities

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
    },
    affinities{
      topBrands{
        name,
        score
      },
      topCategories{
        name,
        score
      },
      topProductTypes{
        name,
        score
      },
      topSkus {
        attribute,
        values{
          name,
          score
        }
      }
    }
  }
}
EOF
```

## Sample Output

```javascript
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
      ],
      "affinities": {
        "topBrands": [
          {
            "name": "Amazing Brand",
            "score": 0.6
          }
        ],
        "topCategories": [
          {
            "name": "/accessories",
            "score": 0.4
          }
        ],
        "topProductTypes": [
          {
            "name": "accessory",
            "score": 0.53
          }
        ],
        "topSkus": [
          {
            "attribute": "size",
            "values": [
              {
                "name": "36",
                "score": 0.5
              }
            ]
          },
          {
            "attribute": "color",
            "values": [
              {
                "name": "green",
                "score": 0.8
              }
            ]
          }
        ]
      }
    }
  }
}
```

