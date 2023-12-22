# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money

## Solution

Sur ce lab il y a un article `Gift Card` qui coûte $10,00.

Quand on s'inscrit à la newsletter, on obtient un code promo de 30% de réduction.

La première vulnérabilité c'est qu'on peut utiliser le code promo plusieurs fois.

Ce qui veut dire qu'on peut acheter une carte cadeau d'une valeur de $10,00 pour $7,00, ce qui nous fait un bénéfice de $3,00.

Répéter l'opération jusqu'à avoir $1 000,00.

Voir la solution du lab pour automatiser avec BS.
