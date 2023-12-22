# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-insufficient-workflow-validation

## Solution

Sur ce lab, quand on veut acheter un article, voici le workflow:

1. On ajoute l'article au panier
2. On clique sur le bouton "Checkout"
3. On est redirigé vers `/cart/order-confirmation?order-confirmed=true` si ca s'est bien passé

Si on skip la deuxième étape, c'est à dire qu'on ajoute la jacket au panier, puis qu'on va directement sur `/cart/order-confirmation?order-confirmed=true`, on peut acheter l'article sans payer.