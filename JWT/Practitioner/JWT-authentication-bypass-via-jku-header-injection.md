# Lab

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection

## Solution

Dans Burp, chargez l'extension JWT Editor depuis le magasin BApp.

Connectez-vous à votre compte dans le lab et envoyez la requête GET `/my-account` post-connexion à Burp Repeater.

Dans Burp Repeater, changez le chemin vers `/admin` et envoyez la requête. Observez que le panneau d'administration n'est accessible qu'en tant qu'utilisateur administrateur.

Accédez à l'onglet JWT Editor Keys dans la barre d'onglets principale de Burp.

Cliquez sur Nouvelle Clé RSA.

Dans la boîte de dialogue, cliquez sur Générer pour créer automatiquement une nouvelle paire de clés, puis cliquez sur OK pour sauvegarder la clé. Notez que la taille de la clé sera mise à jour automatiquement plus tard.

Dans le navigateur, allez sur le serveur d'exploitation.

Remplacez le contenu de la section Corps par un ensemble JWK vide comme suit :

```json
{
    "keys": [

    ]
}
```

De retour sur l'onglet JWT Editor Keys, faites un clic droit sur l'entrée de la clé que vous venez de générer, puis sélectionnez Copier la Clé Publique comme JWK.

Collez le JWK dans le tableau de clés sur le serveur d'exploitation, puis stockez l'exploit. Le résultat devrait ressembler à ceci :

```json
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "893d8f0b-061f-42c2-a4aa-5056e12b8ae7",
            "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw"
        }
    ]
}
```

Revenez à la requête GET `/admin` dans Burp Repeater et passez à l'éditeur de messages JSON Web Token généré par l'extension.

Dans l'en-tête du JWT, remplacez la valeur actuelle du paramètre `kid` par le `kid` du JWK que vous avez téléchargé sur le serveur d'exploitation.

Ajoutez un nouveau paramètre `jku` à l'en-tête du JWT. Définissez sa valeur sur l'URL de votre ensemble JWK sur le serveur d'exploitation.

Dans le payload, changez la valeur de la revendication `sub` en `administrator`.

Au bas de l'onglet, cliquez sur Signer, puis sélectionnez la clé RSA que vous avez générée dans la section précédente.

Assurez-vous que l'option Ne pas modifier l'en-tête est sélectionnée, puis cliquez sur OK. Le jeton modifié est maintenant signé avec la signature correcte.

Envoyez la requête. Observez que vous avez réussi à accéder au panneau d'administration.

Dans la réponse, trouvez l'URL pour supprimer Carlos (`/admin/delete?username=carlos`). Envoyez la requête à ce point de terminaison pour résoudre le lab.