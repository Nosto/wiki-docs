# Updating Identities

Mutations can be used to update the email identities in Nosto. An "identity" is the personal information associated with an email address.

## Upserting Identities

The `upsertIdentity` mutation allows you to upsert the details of an identity. The given example updates the customer attributes for the email john.doe@nosto.com and requests the details of all the attributes of the identity.

If the identity for john.doe@nosto.com does not exist, a new identity will be created.

**Note:** If a specified attribute already exists on that identity, it will be overwritten.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation {
  upsertIdentity(identity: {
    email: "john.doe@nosto.com",
    attributes: [
      {name: "loyalty-tier", value: "Gold"},
      {name: "shoe-size", value: "42"},
    ]
  }) {
    email,
    attributes {
      name,
      value
    }
  }
}
EOF
```

#### What can identity attributes be used for?

The attributes associated with an identity can be used to segment users. This works similarly to how the attributes can be leveraged [when importing them via a CSV](https://help.nosto.com/en/articles/2884483-how-to-import-customer-data-to-nosto-via-api).

## Deleting Identities

There is currently no way to delete an identity.

