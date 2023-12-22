# Lab

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal

## Solution

### Note

Dans cette solution, le paramètre `kid` est dirigé vers le fichier standard `/dev/null`. En pratique, le `kid` peut pointer vers n'importe quel fichier ayant un contenu prévisible.

#### Génération d'une Clé de Signature Appropriée
- Chargez l'extension JWT Editor depuis le magasin BApp dans Burp.
- Connectez-vous à votre compte dans le lab et envoyez la requête GET `/my-account` post-connexion à Burp Repeater.
- Dans Burp Repeater, changez le chemin vers `/admin` et envoyez la requête. Notez que le panneau d'administration est accessible uniquement en tant qu'utilisateur administrateur.
- Allez dans l'onglet JWT Editor Keys de la barre d'onglets principale de Burp.
- Cliquez sur Nouvelle Clé Symétrique.
- Dans la boîte de dialogue, cliquez sur Générer pour créer une nouvelle clé en format JWK. La taille de la clé sera mise à jour automatiquement plus tard.
- Remplacez la valeur générée pour la propriété `k` par un octet nul encodé en Base64 (`AA==`). Notez que c'est un contournement car l'extension JWT Editor ne permet pas de signer des tokens en utilisant une chaîne vide.
- Cliquez sur OK pour sauvegarder la clé.

#### Modification et Signature du JWT
- Revenez à la requête GET `/admin` dans Burp Repeater et passez à l'onglet éditeur de messages JSON Web Token généré par l'extension.
- Dans l'en-tête du JWT, modifiez la valeur du paramètre `kid` pour qu'elle pointe vers le fichier `/dev/null` via une séquence de traversée de chemin :

```
../../../../../../../dev/null
```
- Dans le payload JWT, changez la valeur de la revendication `sub` en `administrator`.
- Au bas de l'onglet, cliquez sur Signer, puis sélectionnez la clé symétrique que vous avez générée précédemment.
- Assurez-vous que l'option Ne pas modifier l'en-tête est sélectionnée, puis cliquez sur OK. Le jeton modifié est maintenant signé en utilisant un octet nul comme clé secrète.
- Envoyez la requête et observez que vous avez réussi à accéder au panneau d'administration.
- Dans la réponse, trouvez l'URL pour supprimer Carlos (`/admin/delete?username=carlos`). Envoyez la requête à ce point de terminaison pour résoudre le lab.