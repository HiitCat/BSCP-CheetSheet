# Lab

https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion

## Solution

### Obtention de la Clé Publique du Serveur
- Chargez l'extension JWT Editor depuis le magasin BApp dans Burp.
- Connectez-vous à votre compte dans le lab et envoyez la requête GET `/my-account` post-connexion à Burp Repeater.
- Dans Burp Repeater, changez le chemin vers `/admin` et envoyez la requête. Notez que le panneau d'administration est accessible uniquement en tant qu'utilisateur administrateur.
- Dans le navigateur, allez à l'endpoint standard `/jwks.json` et observez que le serveur expose un ensemble JWK contenant une seule clé publique.
- Copiez l'objet JWK de l'intérieur du tableau `keys`. Assurez-vous de ne pas copier de caractères de l'array environnant.

## Générer une Clé de Signature Malveillante
- Dans Burp, allez à l'onglet JWT Editor Keys de la barre d'onglets principale.
- Cliquez sur Nouvelle Clé RSA.
- Dans la boîte de dialogue, assurez-vous que l'option JWK est sélectionnée, puis collez le JWK que vous venez de copier. Cliquez sur OK pour sauvegarder la clé.
- Faites un clic droit sur l'entrée de la clé que vous venez de créer, puis sélectionnez Copier la Clé Publique comme PEM.
- Utilisez l'onglet Décodeur pour encoder en Base64 cette clé PEM, puis copiez la chaîne résultante.
- Revenez à l'onglet JWT Editor Keys.
- Cliquez sur Nouvelle Clé Symétrique. Dans la boîte de dialogue, cliquez sur Générer pour créer une nouvelle clé en format JWK. La taille de la clé sera mise à jour automatiquement plus tard.
- Remplacez la valeur générée pour la propriété `k` par le PEM encodé en Base64 que vous venez de créer.
- Sauvegardez la clé.

## Modifier et Signer le Token
- Revenez à la requête GET `/admin` dans Burp Repeater et passez à l'onglet JWT généré par l'extension.
- Dans l'en-tête du JWT, changez la valeur du paramètre `alg` en `HS256`.
- Dans le payload, changez la valeur de la revendication `sub` en `administrator`.
- Au bas de l'onglet, cliquez sur Signer, puis sélectionnez la clé symétrique que vous avez générée précédemment.
- Assurez-vous que l'option Ne pas modifier l'en-tête est sélectionnée, puis cliquez sur OK. Le jeton modifié est maintenant signé en utilisant la clé publique du serveur comme clé secrète.
- Envoyez la requête et observez que vous avez réussi à accéder au panneau d'administration.
- Dans la réponse, trouvez l'URL pour supprimer Carlos (`/admin/delete?username=carlos`). Envoyez la requête à ce point de terminaison pour résoudre le lab.