# Pivoting

El pivoting es una técnica de post-explotación que consiste en utilizar un host comprometido que está conectado a múltiples redes para obtener acceso a sistemas dentro de otras redes. Después de obtener acceso a un host, podemos utilizar el host comprometido para explotar otros hosts de una red interna privada a la que anteriormente no podíamos acceder.

Meterpreter nos proporciona la capacidad de añadir una ruta de red hacia la subred de la red interna, realizar **port forwarding (redirección de puertos)** y, como consecuencia, escanear y explotar otros sistemas de la red. El port forwarding es el proceso de redirigir el tráfico desde un puerto específico de un sistema objetivo hacia un puerto específico de nuestro sistema.

En pivoting, podemos redirigir un puerto remoto de un host al que anteriormente no teníamos acceso hacia un puerto local de nuestro sistema Kali Linux, de modo que podamos interactuar remotamente con el servicio que se está ejecutando en ese puerto o explotarlo.

El primer paso es escanear la red en busca de otros hosts. Para ello necesitamos primero localizar las interfaces y sus direcciones IP, además de establecer una ruta para conectar nuestra máquina kali con la red interna.

```bash
meterpreter > ipconfig
meterpreter > autoroute -s subred/mascara # Por ejemplo: 192.168.1.0/24
```

La opción `-s` indica la subred. Podemos ver la lista de rutas creadas con:

```bash 
meterpreter > run autoroute -p
``` 

A continuación, para escanear la red interna pausamos la sesión y usamos el módulo `portscan/tcp`:

```bash
meterpreter > background
msf > use auxiliary/scanner/portscan/tcp
msf auxiliary(scanner/portscan/tcp) > set rhosts <ip_pivoting>
msf auxiliary(scanner/portscan/tcp) > run
```

Esto nos permite comprobar los puertos abiertos, pero no enumerar más informacion. Para ello necesitamos usar nmap, pero debemos establecer una redirección de puertos previamente.

```bash
meterpreter > portfwd -l 1234 -p 80 -r 192.168.1.10
```

| Opción            | Significado                                  |
| ----------------- | -------------------------------------------- |
| `portfwd`         | Gestiona una redirección de puertos.         |
| `-l 1234`         | Puerto **local** de kali donde se escuchará. |
| `-p 80`           | Puerto **remoto** al que se redirigirá.      |
| `-r 192.168.1.10` | Host remoto al que se enviará el tráfico.    |

Como el puerto ahora es accesible desde nuesta máquina kali, podemos escanearlo de forma normal con nmap teniendo en cuenta que es accesible desde localhost:

```bash
nmap -sC -sV -p 1234 localhost
```

Una vez terminada la enumeración y el análisis de vulnerabilidades del segundo host podemos explotarlo de la fomrma habitual, ya que msfconsole está configurado para establecer una ruta a esta máquina a traves del primer host.
