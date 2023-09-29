# Lab

https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-dom-xss-via-client-side-prototype-pollution

## Solution

Activer le prototype pollution dans DOM Invader.

Rafraichir la page.

Trouver le point d'entrer et scanner pour un gadget.

Créer l'exploit avec DOM Invader : `https://0a5c00c804a51dc480ecb77a0021007a.web-security-academy.net/?__proto__[transport_url]=data%3A%2Calert%281%29`