# Lab

https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references

## Solution

Il y a une fonctionnalité de chat en ligne sur le site.

Quand on y accède, on voit un bouton `Download transcript` qui permet de télécharger le transcript du chat.

Si on télécharge le transcript, on voit que le fichier est nommé `2.txt` et que le contenu vient de la requête `GET /download-transcript/2.txt`.

En changeant le numéro dans l'URL, on peut accéder aux transcripts des autres utilisateurs.

Avec le transcript `1.txt` on peut trouver le mot de passe de `carlos` et se connecter avec son compte.