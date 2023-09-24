# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-param

## Lab: Web cache poisoning via an unkeyed query parameter

### Lab : Exploitation de Cache Oracle pour Injection XSS

1. **Observation du Cache Oracle :**
   - Chargez la page d'accueil du site web et notez qu'elle est un oracle de cache approprié.
   - Observez que chaque fois que vous modifiez la chaîne de requête (`query string`), vous obtenez un "cache miss", indiquant qu'elle fait partie de la clé de cache.
   - Notez également que la chaîne de requête est reflétée dans la réponse.

2. **Ajout d'un Cache Buster :**
   - Ajoutez un paramètre de "cache buster" à votre requête.

    ```http
    GET /?cache_buster=12345
    ```

3. **Identification de Paramètres :**
   - Utilisez la fonction "Guess GET parameters" de Param Miner pour identifier que le paramètre `utm_content` est pris en charge par l'application.

4. **Confirmation du Paramètre Non-Clé (Unkeyed) :**
   - Ajoutez `utm_content` à la chaîne de requête et vérifiez que vous obtenez toujours un "cache hit".

    ```http
    GET /?utm_content=test
    ```

   - Continuez à envoyer la requête jusqu'à obtenir un "cache miss".
   - Observez que ce paramètre non-clé est également reflété dans la réponse.

5. **Injection XSS :**
   - Envoyez une requête avec un paramètre `utm_content` qui permet de sortir de la chaîne reflétée et injecte une charge utile XSS.

    ```http
    GET /?utm_content='/><script>alert(1)</script>
    ```

   - Une fois que votre payload est mis en cache, retirez le paramètre `utm_content`, faites un clic droit sur la requête et sélectionnez "Copy URL". 

6. **Validation de l'Exploit :**
   - Ouvrez cette URL dans le navigateur et vérifiez que `alert()` est déclenché lors du chargement de la page.

7. **Empoisonnement du Cache :**
   - Retirez votre "cache buster", ajoutez à nouveau le paramètre `utm_content` avec votre payload, et rejouez la requête jusqu'à ce que le cache soit empoisonné pour les utilisateurs normaux.

    ```http
    GET /?utm_content='/><script>alert(1)</script>
    ```

8. **Résolution du Lab :**
   - Le lab sera résolu lorsque l'utilisateur victime visitera la page d'accueil empoisonnée.