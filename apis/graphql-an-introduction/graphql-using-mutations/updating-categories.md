# Updating Categories

Mutations can be used to update the category listing in Nosto. The `upsertCategories` mutation allows you to update one or more category at one go. If the category doesn't already exist, a new one is created.

A category can have the following fields:

* `id` The category identifier. If a category with this id doesn't already exist, a new one is created.
* `name` The displayed name for the category.
* `urlPath` The path that can be used to generate the URL of the category listing.
* `available` If the category is visible on the store. This can be set to false to soft delete the category in an upsert.

Some stores support hierarchical categories i.e. a category may have parent and child categories. The following fields can be used for hierarchical categories:

* `parentId` The identifier of the parent category.
* `fullName` The name of every category in the hierarchy.

```
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation {
  upsertCategories(categories: [{
    id: "123",
    name: "Shoes",
    parentId: "456",
    urlPath: "/categories/mens/shoes",
    fullName: "/Mens/Shoes",
    available: true
  }]) {
    categoryResult {
      errors {
        field
        message
      }
      category {
        id
        name
        parentId
        urlPath
        fullName
        available
      }
    }
  }
}
EOF
```
