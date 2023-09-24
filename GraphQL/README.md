# GraphQL

## Introspection

### Basique 

```graphql
{
    "query":"query {__schema{types{name, fields{name, type{name}}}}}"
}
```

### Sur les Query

```graphql
{
    "query":"query {__schema{queryType{name, fields{name, args{name, type{name}}}}}}"
}
```