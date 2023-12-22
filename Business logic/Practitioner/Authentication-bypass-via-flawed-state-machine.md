# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-flawed-state-machine

## Solution

Complétez le processus de connexion et remarquez que vous devez sélectionner votre rôle avant d'accéder à la page d'accueil.

Utilisez l'outil de découverte de contenu pour identifier le chemin `/admin`.

Accéder directement à `/admin` depuis la page de sélection de rôle et observez que cela ne fonctionne pas.

Déconnectez-vous, puis retournez à la page de connexion.

Dans Burp, activez l'interception du proxy, puis connectez-vous.

Transmettez la requête POST `/login`. La requête suivante sera GET `/role-selector`. Supprimez cette requête.

Ensuite, accédez à la page d'accueil du laboratoire. Remarquez que votre rôle a été automatiquement défini comme administrateur, vous donnant ainsi accès au panneau d'administration.

Pour résoudre le laboratoire, supprimez `carlos`.