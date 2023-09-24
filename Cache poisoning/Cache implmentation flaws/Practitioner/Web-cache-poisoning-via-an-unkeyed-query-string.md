# Lab

https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-query

### Lab : Exploitation de l'Oracle de Cache

1. **Initialisation de Burp et chargement de la page d'accueil :**
   - Ouvrez Burp Suite et chargez la page d'accueil du site web.
   - Allez dans `Proxy > Historique HTTP` pour trouver la requête `GET` de la page d'accueil.

2. **Analyse du Cache :**
   - Envoyez la requête à `Burp Repeater`.
   - Ajoutez des paramètres de requête arbitraires pour tester le cache.

    ```http
    GET /?test_param=test_value
    ```

   - Notez si vous obtenez un "cache hit" ou un "cache miss".

3. **Utilisation du Cache Buster :**
   - Ajoutez l'en-tête `Origin` pour casser le cache.

    ```http
    Origin: https://example.com
    ```

4. **Injection de Paramètres :**
   - Observez que les paramètres injectés sont reflétés dans la réponse lorsque vous obtenez un "cache miss".

5. **Injection XSS :**
   - Injectez une charge utile XSS en utilisant un paramètre qui brise la chaîne reflétée.

    ```http
    GET /?evil='/><script>alert(1)</script>
    ```

   - Rejouez la requête jusqu'à voir "X-Cache: hit" dans les en-têtes et que votre payload soit reflétée dans la réponse.

6. **Simulation de la Victime :**
   - Retirez la chaîne de requête et utilisez le même "cache buster". Vérifiez que la réponse mise en cache contient toujours votre payload.

7. **Empoisonnement du Cache :**
   - Retirez l'en-tête `Origin` et ajoutez votre payload à la chaîne de requête.
   - Continuez à rejouer la requête jusqu'à empoisonner le cache.

    ```http
    GET /?evil='/><script>alert(1)</script>
    ```

8. **Confirmation de l'Attaque :**
   - Ouvrez la page d'accueil dans un navigateur et vérifiez que la fenêtre popup s'affiche.

9. **Résolution du Lab :**
   - Le lab sera considéré comme résolu lorsque la victime accédera à la page d'accueil empoisonnée. Vous pourrez devoir réempoisonner le cache si le lab n'est pas résolu après 35 secondes.