# Lab

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-on-debug-page

## Payload

On sait qu'il y a une page de debug, on a juste a la trouver.

Dans la réponse de la home page, on trouve un lien en commentaire:

```html
<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->
```

On peut donc aller sur la page `/cgi-bin/phpinfo.php` et on trouve la variable `SECRET_KEY` qui vaut `j0qibgm90uz4xz5ao4rmpc5zf3e2fk6k`.