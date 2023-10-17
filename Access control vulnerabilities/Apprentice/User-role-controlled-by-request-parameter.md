# Lab

https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter

## Solution

Lorsqu'on se connecte et qu'on essaie d'accéder à la page d'administration, on a une erreur : `Admin interface only available if logged in as an administrator`.

Si on regarde de plus près la requête envoyée lorsqu'on accède à `/admin`, on voit le cookie `Admin=false`.

En modifiant ce cookie à la valeur `true`, on a accès à la page d'administration et on peut supprimer l'utilisateur `carlos` via une requête à l'URL `/admin/delete?username=carlos`.