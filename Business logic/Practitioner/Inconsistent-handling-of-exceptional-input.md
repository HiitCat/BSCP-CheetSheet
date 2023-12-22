# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-handling-of-exceptional-input

## Solution

Pour valider ce lab on doit accéder à l'admin panel, et pour accéder à cet admin panel, il faut un compte avec une adresse mail `@dontwannacry.com`.

Quand on créé un compte avec une adresse mail qu'on possède, on reçoit un lien de confirmation pour valider le compte ce qui nous empêche de créer un compte directement avec une adresse mail `@dontwannacry.com`.

En revanche lorsqu'on créé un compte avec un mail très long de plus de 300 caractères, on se rend compte que l'email est tronqué à 255 caractères.

On peut abuser de cette troncature pour créer un compte avec une adresse mail qui fini en `@dontwannacry.com`.

Par exemple : `aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa@dontwannacry.com.exploit-0a0700cb045bae4e82e4420a014500cb.exploit-server.net`

Cette adresse mail se fait tronquer à `aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa@dontwannacry.com`

Mais on recevra un mail de confirmation à `@dontwannacry.com.exploit-0a0700cb045bae4e82e4420a014500cb.exploit-server.net` qui est un domaine que l'on contrôle grâce à l'exploit server.