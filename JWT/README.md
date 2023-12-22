# JWT

## Attaques

### Signature non vérifiée

Parfois, les applications ne vérifient pas la signature du JWT, et donc il suffit de modifier le contenu du JWT pour avoir accès à des fonctionnalités auxquelles on n'est pas censé avoir accès.

### Algorithme none

Certaines applications acceptent l'algorithme `none` pour la signature du JWT.

```json
{
    "typ": "JWT",
    "alg": "none"
}
```

### Secret faible

Parfois, les applications utilisent un secret faible pour signer le JWT, ce qui permet de le casser.

On peut utiliser `hashcat` pour casser le secret.

```bash
hashcat -a 0 -m 16500 jwt.txt rockyou.txt --show
hashcat -a 0 -m 16500 [JWT] rockyou.txt --show
```

Voici une liste de secrets faibles : [wallarm/jwt-secrets](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list)