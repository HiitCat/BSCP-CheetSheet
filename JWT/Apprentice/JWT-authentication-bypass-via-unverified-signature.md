# Lab

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature

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

Si on change le body pour mettre en `sub` le nom `administrator`, le token est valide car la signature n'est pas vérifiée.

Ensuite on a accès à la page d'administration et on peut supprimer l'utilisateur `carlos` pour valider le lab.

