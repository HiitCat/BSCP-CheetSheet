# Lab

https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url

## Solution

Il y a un admin panel sur le site qui n'est pas référencé dans le fichier `robots.txt`.

En regardant les sources de la page d'accueil on tombe sur ce script JS :

```js
var isAdmin = false;
if (isAdmin) {
   var topLinksTag = document.getElementsByClassName("top-links")[0];
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-5456tx');
   adminPanelTag.innerText = 'Admin panel';
   topLinksTag.append(adminPanelTag);
   var pTag = document.createElement('p');
   pTag.innerText = '|';
   topLinksTag.appendChild(pTag);
}
```

Le script ajoute un lien vers le paneau d'administration si l'utilisateur est admin.

Ce qui nous révèle le lien du panel : `/admin-5456tx`