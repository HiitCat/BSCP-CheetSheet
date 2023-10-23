## PHP Deserialisation

Exemple : `O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}`

- `O:4:"User"` : Un objet avec un nom de classe de 4 caractères "User"
- `2` : L'objet a deux propriétés
- `s:4:"name"` : Le nom du premier attribut est composé de 4 caractères "name"
- `s:6:"carlos"` : La valeur de l'attribut est composé de 6 caractères "carlos"
- `s:10:"isLoggedIn"` : Le nom du second attribut est composé de 10 caractères "isLoggedIn"
- `b:1` : La valeur de l'attribut est un booléen à true

## Fonctions de sérailisation et désérialisation

- `serialize()` : Retourne une chaîne de caractères contenant une représentation sérialisée de la valeur passée en paramètre.
- `unserialize()` : Retourne la valeur initiale d'une variable qui a été sérialisée.

## Java Deserialisation

La sérialisation en Java n'est pas aussi facilement lisible que la sérialisation en PHP.

Cependant, il y a quelques indices qui nous indiquent que nous avons affaire à une sérialisation Java.

- `rO0` : C'est un en-tête de sérialisation Java encodé en base64.
- `ac ed` : C'est un en-tête de sérialisation Java encodé en hexadécimal.

## Fonctions de sérailisation et désérialisation

- `ObjectInputStream.readObject()` : Lit un objet à partir du flux d'entrée.
- `ObjectOutputStream.writeObject()` : Écrit un objet dans le flux de sortie.