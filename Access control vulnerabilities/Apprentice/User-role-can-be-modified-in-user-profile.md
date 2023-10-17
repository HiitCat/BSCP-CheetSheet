# Lab

https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile

## Solution

Le lab dispose d'un panel admin à l'URL `/admin`.

Depuis la description du lab on peut lire : `Le panel est disponible uniquement aux utilisateurs ayant le roleid 2.`.

Sur la page du profil, on a la possibilité de changer son email via un appel API en POST à `/my-account/change-email` et le body JSON :

```json
{
    "email": "itworks@mm.com"
}
```

Si on change le body pour essayer de changer la valeur de `roleid` à `2`, ça fonctionne et on voit maintenant le lien `Admin panel` :

```json
{
    "email": "itworks@mm.com",
    "roleid": 2
}
```

On peut supprimer l'utilisateur `carlos`.