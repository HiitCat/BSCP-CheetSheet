# Lab

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect

## Solution

Si on essaie d'accéder à `/my-account?id=carlos` on a accès au compte de Carlos mais on est redirigé vers la page de connexion.

Par contre quand on regarde les requêtes dans Burp, on voit qu'avant d'être redirigé, on a accès à la page `/my-account` avec les informations de Carlos.