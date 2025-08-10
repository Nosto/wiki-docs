# Using Queries

The query methods allow you to fetch basic recommender data and does not require any sessions. Query operations are light and do not give you the benefit of personalization but can be used to fulfill simple use cases e.g. an in-store display that always shows the top 10 viewed products.

\_The given example simply fetches products related to a given search term aliased as `q_related` and also fetches the most viewed products within the last week.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: false, image: VERSION_7_200_200) {
    hot_now: toplist(hours: 168, sort: VIEWS, params: {
      minProducts: 1
      maxProducts: 10
    }) {
      primary {
        name
        productId
      }
    }
    q_related: search(term: "black", params: {
      minProducts: 1
      maxProducts: 10
    }) {
      primary {
        name
        productId
      }
    }
  }
}
EOF
```

