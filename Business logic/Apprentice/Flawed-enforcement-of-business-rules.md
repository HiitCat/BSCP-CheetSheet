# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-flawed-enforcement-of-business-rules

## Solution

Quand on arrive sur le lab, on nous donne un coupon de reduction pour les nouveaux utilisateurs : `NEWCUST5` qui donne $5 de réduction.

En bas de la page d'accueil il y a un lien pour s'inscrire à la newsletter qui donne un coupon de réduction de 30% : `SIGNUP30`.

Lorsqu'on applique un coupon de réduction, on ne peut pas l'ajouter deux fois de suite.

Par contre, si on ajoute un premier coupon, puis un deuxième, on peut ajouter à nouveau le premier coupon et ainsi de suite.

```
Name	Price	Quantity	
Lightweight "l33t" Leather Jacket	$1337.00	1	
NEWCUST5	-$5.00		
SIGNUP30	-$401.10		
NEWCUST5	-$5.00		
SIGNUP30	-$401.10		
NEWCUST5	-$5.00		
SIGNUP30	-$401.10		
NEWCUST5	-$5.00		
SIGNUP30	-$401.10
```

Ce qui fait un panier à `$0.00`.