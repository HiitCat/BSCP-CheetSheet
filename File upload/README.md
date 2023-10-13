# File upload

## .htaccess

Autoriser l'execution de php dans un dossier via le fichier `.htaccess` :

```bash
AddType application/x-httpd-php .jpg
```

## Bypass l'extension

- Sensible à la casse : `shell.php` -> `shell.PHP`
- Multiple extensions : `shell.php.jpg`
- Ajoute un caractère de fin : `shell.php.`
- URL encode : `shell%2Ephp`
- Ajouter un point-virgule : `shell.php;.png`
- Null byte : `shell.php%00.png`
- Recursif : `shell.p.phphp`

Plus sur [HackTricks](https://book.hacktricks.xyz/pentesting-web/file-upload)