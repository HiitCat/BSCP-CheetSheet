# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-low-level

## Solution

Dans ce lab la gestion des nombres n'est pas faite correctement.

Quand on dépasse la taille maximale d'un `int` (2 147 483 647), le nombre passe en négatif et remonte vers 0.

```
Lightweight "l33t" Leather Jacket	$1337.00	 32123 	
Inflatable Holiday Home	            $34.07	     36 	
```

Fait un panier total de $4.56.