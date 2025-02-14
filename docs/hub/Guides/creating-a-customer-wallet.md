---
sidebar_position: 3
---

# Creating a Customer & Wallet

## Creating a wallet

Creating a wallet on Holaplex Hub involves two main steps: creating a customer and creating a wallet for that customer. The GraphQL mutations required for these steps are shown in the code snippet below →

```graphql
mutation CreateCustomer($input: CreateCustomerInput!) {
  createCustomer(input: $input) {
    customer {
      id
    }
  }
}

mutation CreateCustomerWallet($input: CreateCustomerWalletInput!) {
  createCustomerWallet(input: $input) {
    wallet {
      address
    }
  }
}
```

In this guide, we'll walk you through the process of creating a wallet on Holaplex Hub.

## Prerequisites

- A Holaplex Hub account
- Access to the Holaplex Hub GraphQL API (an access token can be generated on Hub's "Credentials" page)
- A GraphQL client, such as [Apollo Client](https://www.apollographql.com/client/) or a tool like [GraphQL Playground](https://github.com/graphql/graphql-playground)

## Step 1: Create a Customer

The first step is creating a customer on the Holaplex Hub platform. To do this, you need to send a `createCustomer` mutation with the required input parameters.

### Mutation

```graphql
mutation CreateCustomer($input: CreateCustomerInput!) {
  createCustomer(input: $input) {
    customer {
      id
    }
  }
}
```

### Input Parameters

- `$input`: A `CreateCustomerInput` object containing the project `UUID` where you want the customer to be assigned to.

Example:

```json
{
  "input": {
    "project": "<project-id>"
  }
}
```

The `project-id` can be found in the URL of a project page on Hub. For example, if your Hub project page URL is `https://hub.holaplex.com/projects/a56e7745-37a2-40b7-9d25-d5c20b6fc137`, then the corresponding `project-id` is `'a56e7745-37a2-40b7-9d25-d5c20b6fc137'`.

### Example Request

```graphql
mutation {
  createCustomer(input: { project: "<project-id>" }) {
    customer {
      id
    }
  }
}
```
CURL:
```
curl 'https://api.holaplex.com/graphql' -H 'Accept-Encoding: gzip, deflate, br' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Connection: keep-alive' -H 'DNT: 1' -H 'Origin: file://' -H 'Authorization: ACCESS-TOKEN' --data-binary '{"query":"mutation CreateCustomer($input: CreateCustomerInput!) {\n  createCustomer(input: $input) {\n    customer {\n      id\n    }\n  }\n}\n","variables":{"input":{"project":"PROJECT-ID"}}}' --compressed
```
Replace `ACCESS-TOKEN` and `PROJECT-ID`

### Example Response

```json
{
  "data": {
    "createCustomer": {
      "customer": {
        "id": "15433469-a6e2-431b-9eee-be40056e2e0b"
      }
    }
  }
}
```

## Step 2: Create a Wallet for the Customer

After creating a customer, the next step is to create a wallet for that customer using the `createCustomerWallet` mutation.

### Mutation

```graphql
mutation CreateCustomerWallet($input: CreateCustomerWalletInput!) {
  createCustomerWallet(input: $input) {
    wallet {
      address
    }
  }
}
```

### Input Parameters

- `$input`: A `CreateCustomerWalletInput` object containing the customer ID from previous step response, and the desired `assetType` for the wallet.

> Check out the list of supported asset types on the [enums documentation](../../api/enums/asset-type.mdx)

Example:

```json
{
  "input": {
    "customer": "<customer-id>",
    "assetType": "SOL_TEST"
  }
}
```

### Example Request

```graphql
mutation {
  createCustomerWallet(
    input: { customer: "customer-id", assetType: SOL_TEST }
  ) {
    wallet {
      assetId
      address
    }
  }
}
```
CURL:
```
curl 'https://api.holaplex.com/graphql' -H 'Accept-Encoding: gzip, deflate, br' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Connection: keep-alive' -H 'DNT: 1' -H 'Origin: file://' -H 'Authorization: ACCESS-TOKEN' --data-binary '{"query":"mutation CreateCustomerWallet($input: CreateCustomerWalletInput!) {\n  createCustomerWallet(input: $input) {\n    wallet {\n      address\n    }\n  }\n}","variables":{"input":{"customer":"CUSTOMER-ID","assetType":"SOL"}}}' --compressed
```
Replace `ACCESS-TOKEN` and `CUSTOMER-ID`

### Example Response

```json
{
  "data": {
    "createCustomerWallet": {
      "wallet": {
        "assetId": "SOL",
        "address": "wallet-address"
      }
    }
  }
}
```

After completing these two steps, you will have successfully created a wallet for the customer on Holaplex Hub.
The wallet address can be used for minting NFTs, trading, and other wallet-related operations.

Note to find a customer's wallet address, perform the following query, e.g.:
```
{
  project(id:"a56e7745-37a2-40b7-9d25-d5c20b6fc137") {
		name
    customer(id:"33dedde4-543d-4653-bc10-db0a38e719cc") {
      wallet {
        address
      }
    }
  }
}
```
CURL:
```
curl 'https://api.holaplex.com/graphql' -H 'Accept-Encoding: gzip, deflate, br' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Connection: keep-alive' -H 'DNT: 1' -H 'Origin: file://' -H 'Authorization: ACCESS-TOKEN' --data-binary '{"query":"{\n  project(id:\"PROJECT-ID\") {\n\t\tname\n    customer(id:\"CUSTOMER-ID\") {\n      wallet {\n        address\n      }\n    }\n  }\n}"}' --compressed
```
Replace `ACCESS-TOKEN`, `PROJECT-ID`, and `CUSTOMER-ID`