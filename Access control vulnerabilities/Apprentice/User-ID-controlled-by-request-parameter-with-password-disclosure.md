# Lab

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure

## Solution

Quand on se connecte avec `wiener:peter` on est redirigé vers `/my-account?id=wiener`.

Si on change le paramètre `id` par `administrator` on a accès au compte de l'admin.

Sur la page `my-account` on peut voir le mot de passe de l'admin dans le champ password qui est censéré puisqu'en `type="password"`.

On peut le récupérer en changeant le type du champ en `text` avec les DevTools ou juste en regardant le code source de la page.

Avec ce mot de passe on peut se connecter avec le compte `administrator` et accéder au panel d'admin pour supprimer le compte `carlos`.