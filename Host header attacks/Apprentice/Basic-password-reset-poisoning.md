# Lab

https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning/lab-host-header-basic-password-reset-poisoning

## Solution

Sur ce lab, il suffit d'intercepeter la requête de réinitialisation du mot de passe et de modifier le header `Host` pour qu'il pointe vers notre serveur.

On utiliser Burp Collaborator pour récupérer le token de réinitialisation du mot de passe.

Ensuite en vistant l'URL suivante, on peut réinitialiser le mot de passe de l'utilisateur `carlos`: `/forgot-password?temp-forgot-password-token=78b5nhgjdf3o15wzr3adi01aebw9n2vk`.