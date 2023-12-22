# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-security-controls

## Solution

Dans ce lab le but est d'accéder à l'admin panel pour supprimer l'utilisateur `carlos`.

Pour accéder au panel admin, il faut que notre compte ait une adresse mail `@dontwannacry.com`.

On peut créer un compte mais un email de confirmation est envoyé pour valider le compte.

On peut faire un compte avec une adresse mail qu'on possède et ensuite la modifier via `My Account` pour accéder au panel admin.