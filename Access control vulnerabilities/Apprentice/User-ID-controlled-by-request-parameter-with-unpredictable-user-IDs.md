# Lab

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids

## Solution

L'URL pour accéder au détail d'un compte n'est pas protégée par un contrôle d'accès.

On peut y accéder si on a le GUID de l'utilisateru, par exemple : `/my-account?id=af1a3215-b1f2-4087-bd1f-cf24079139f7`

Etant donné qu'on ne peut pas deviner le GUID d'un utilisateur, on va essayer de le trouver.

Sur le blog, on voit que chaque article est posté par un utilisateur avec son GUID dans l'URL.

On peut trouver un post fait par carlos et ainsi trouver son GUID.