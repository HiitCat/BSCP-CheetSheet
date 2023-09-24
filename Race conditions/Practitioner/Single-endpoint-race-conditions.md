# Lab

https://portswigger.net/web-security/race-conditions/lab-race-conditions-single-endpoint

## Payload

Le but est de changer son adresse mail tout en envoyant le mail de validation à notre adresse.

Dans le repeater j'ajoute ces 2 requetes :

- Modification de l'adresse mail pour `wiener@exploit-0a5b00370314503580ca9e7c01c500f4.exploit-server.net`
- Modification de l'adresse mail pour `carlos@ginandjuice.shop` (x3)

On recoit sur la boite mail `wiener@exploit-0a5b00370314503580ca9e7c01c500f4.exploit-server.net` un lien pour changer son email vers `carlos@ginandjuice.shop`.

On a l'adresse mail `carlos@ginandjuice.shop` et on a maintenant accès à la page admin qui nous permet de supprimer le compte de Carlos.