# Lab

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter

## Solution

L'URL pour accéder au compte `/my-account` prend un query parameter `id` qui est le username de l'utilisateur.

Par défaut, quand on se connecte avec `wiener:peter` on est redirigé vers `/my-account?id=wiener`.

Si on essaie d'accéder à `/my-account?id=carlos` on a accès au compte de Carlos.