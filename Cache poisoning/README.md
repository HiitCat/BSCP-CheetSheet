## Afficher la clé de cache

Des fois il est possible d'utiliser le header `Pragma: x-get-cache-key` pour afficher dans les headers de réponse la clé de cache.

## Les paramètres UTM

Les paramètres UTM sont des paramètres utilisés par Google Analytics pour permettre de suivre les utilisateurs. Ils sont souvent utilisés dans les liens de campagnes publicitaires.

On retrouve souvent les paramètres suivants:

- utm_source
- utm_medium
- utm_campaign
- utm_term
- utm_content

## Fat GET

Le fat GET est une technique qui permet de faire des requêtes GET avec un body. Cela permet de contourner les protections qui bloquent les requêtes GET avec un body.

Si le body bloque, il est des fois possible de contourner la protection en utilisant le header `X-HTTP-Method-Override: POST` pour indiquer que la requête est en réalité une requête POST tout en gardant la méthode GET.