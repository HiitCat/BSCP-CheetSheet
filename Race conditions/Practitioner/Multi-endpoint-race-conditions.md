# Lab

https://portswigger.net/web-security/race-conditions/lab-race-conditions-multi-endpoint

## Payload

Le but est de payer le panier et pendant le process de validation, ajouter un autre article.

Dans le repeater j'ajoute ces 4 requetes :

- Appel à la page d'acceuil pour warmer le backend
- Ajout d'un article pas chère dans le panier
- Validation du panier
- Ajout d'un article plus chère dans le panier x20