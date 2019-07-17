## Download Schema

To get the `schema.graphql` I used [this CLI](ttps://github.com/graphql-cli/graphql-cli)

Also I had some minor issue getting the scheme behide some authenticated server. So, I found this [issue](https://github.com/prisma/vscode-graphql/issues/68)

Which uses this json

```json
{
  "projects": {
    "myProject": {
      "schemaPath": "schema.graphql",
      "extensions": {
        "endpoints": {
          "local": {
            "url": "http://localhost:8000/app_dev.php/graphql",
            "headers": {
              "Authorization": "Bearer ${env:API_TOKEN}"
            }
          }
        }
      }
    }
  }
}
```

So, all I did was update my `.graphqlconfig` to:

```json
{
  "projects": {
    "server-mobile": {
      "schemaPath": "schema.graphql",
      "extensions": {
        "endpoints": {
          "prod": {
            "url": "https://server.com/queries",
            "headers": {
              "Authorization": "Bearer token"
            }
          }
        }
      }
    }
  }
}
```
