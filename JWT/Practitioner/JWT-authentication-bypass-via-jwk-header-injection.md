# Lab

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jwk-header-injection

## Solution

Dans Burp, commencez par charger l'extension JWT Editor depuis le magasin BApp.

Une fois connecté à votre compte dans le lab, envoyez la requête GET `/my-account` post-connexion à Burp Repeater.

Dans Burp Repeater, modifiez le chemin vers `/admin` et envoyez la requête. Notez que le panneau d'administration n'est accessible qu'en étant connecté en tant qu'utilisateur administrateur.

Accédez à l'onglet JWT Editor Keys dans la barre d'onglets principale de Burp.

Cliquez sur Nouvelle Clé RSA.

Dans la boîte de dialogue, cliquez sur Générer pour créer automatiquement une nouvelle paire de clés, puis cliquez sur OK pour sauvegarder la clé. Remarquez que vous n'avez pas besoin de sélectionner une taille de clé, car cela sera automatiquement mis à jour plus tard.

Revenez à la requête GET `/admin` dans Burp Repeater et passez à l'onglet JSON Web Token généré par l'extension.

Dans le payload, changez la valeur de la revendication `sub` en `administrator`.

Au bas de l'onglet JSON Web Token, cliquez sur Attaque, puis sélectionnez Clé JWK Embarquée. Lorsqu'on vous le demande, sélectionnez votre nouvelle clé RSA générée et cliquez sur OK.

Dans l'en-tête du JWT, observez qu'un paramètre `jwk` a été ajouté contenant votre clé publique.

Envoyez la requête. Observez que vous avez réussi à accéder au panneau d'administration.

Dans la réponse, trouvez l'URL pour supprimer Carlos (`/admin/delete?username=carlos`). Envoyez la requête à ce point de terminaison pour résoudre le lab.