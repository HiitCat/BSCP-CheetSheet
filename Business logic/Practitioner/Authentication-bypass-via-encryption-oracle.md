# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle

## Solution

En se connectant avec l'option `Rester connecté` activée, on observe via Burp que le cookie de connexion est crypté.

Lorsqu'on soumet un commentaire avec une adresse email invalide, le serveur crée un cookie de notification crypté et redirige, affichant le message d'erreur `Invalid email address: your-invalid-email`.

Ce comportement suggère que l'email est décrypté à partir du cookie de notification.

En utilisant Burp Repeater pour envoyer des requêtes `POST` et `GET`, on peut encrypter et décrypter des données.

On renomme les onglets en `encrypt` et `decrypt` pour simplifier.

En copiant le cookie `Rester connecté` dans le cookie de notification et en envoyant la requête, la réponse révèle le format du cookie comme `username:timestamp`. On change ensuite le paramètre `email` pour générer un nouveau cookie de notification.

Ce processus montre que tout ce qui est passé via le paramètre `email` ajoute automatiquement le préfixe `Invalid email address: `. Après avoir décodé le cookie (URL et Base64) via Burp Decoder et supprimé les premiers 23 octets, on obtient un format compatible avec l'algorithme de chiffrement par blocs utilisé, qui exige une longueur multiple de 16.

Pour finaliser, on ajoute 9 caractères au début de la valeur du cookie voulu et on crypte cette entrée. Le décryptage du nouveau cookie réussit, sans le préfixe `Invalid email address: `. Ensuite, on se connecte en tant qu'administrateur en envoyant une requête `GET /` avec le cookie de session modifié, ce qui donne accès au panneau d'administration.

Enfin, on navigue vers `/admin` et on résout le lab en allant à `/admin/delete?username=carlos`, exploitant ainsi l'option de suppression d'utilisateurs.