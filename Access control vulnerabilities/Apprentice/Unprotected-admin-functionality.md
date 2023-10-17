# Lab

https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

## Solution

Le lab possède un panel administrateur qui n'est pas protégé.

On peut retrouver l'URL du panel dans le fichier `/robots.txt` :

```
User-agent: *
Disallow: /administrator-panel
```