# Lab

https://portswigger.net/web-security/host-header/exploiting/lab-host-header-authentication-bypass

## Solution

Sur ca lab, si on change le header `Host` de la requête d'accès à la page d'accueil, on peut voir qu'il y a un lien vers la page d'administration.

On peut y accéder en cliquant sur le lien ou en visitant l'URL suivante: `/admin` tout en modifiant le header `Host` de la requête.

Ensuite on a directement accès à la suppression de l'utilisateur `carlos` et on peut valider le lab.