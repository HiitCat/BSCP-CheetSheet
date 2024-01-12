# Lab

https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-request-splitting-via-crlf-injection

## Solution

L'objectif de ce lab est d'empoisoinner la queue de requêtes avec une 404 non trouvée pour provoquer une redirection vers la page d'accueil.

Ici, on va ajouter une requête à la liste des requêtes à traiter par le back-end de façon à ce que nous récupérons la réponse de redirection à la place de l'admin dans le but de récupérer le cookie.

Pour cela, on va ajouter un header `Foo` à notre request `GET / HTTP/1.1` qui va être traité par le front-end et qui va être envoyé au back-end.

La valeur de cet header sera `bar\r\nGET /x HTTP/1.1\r\nHost:0acc003f0326e20c828a3d530096008e.web-security-academy.net`.

Ainsi, le back-end va traiter la requête `GET /x HTTP/1.1` et va renvoyer une 404 non trouvée.

L'objectif est que cette réponse 404 soit envoyée à l'admin et que nous, attaquants, récupérons la réponse de redirection contenant le cookie.

![HTTP-2-request-splitting-via-CRLF-injection](https://portswigger.net/web-security/request-smuggling/advanced/images/stealing-other-users-responses.jpg)