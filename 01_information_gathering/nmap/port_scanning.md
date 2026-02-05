# Escaneo de puertos con *Nmap*

La función principal de *Nmap* en el escaneo de puertos es determinar cuales se encuentran abiertos. El estado de los puertos analizados puede ser: abierto, cerrado o filtrado (no se puede determinar su estado porque está filtrado por un firewall). *Nmap* tiene diferentes tipos de técnicas para escaneo de puertos:

- ***TCP SYN scan***: es rápido y sigiloso. Igual que en *host discovery (-PS)*, no completa la conexión.
- ***Connect scan***: completa la conexión TCP SYN.
- ***TCP ACK scan***: similar a *TCP ACK*. Útil para descubrir si hay firewalls filtrando el tráfico.
- ***UDP Scan***: Busca puertos UDP abiertos. Suelen ser más difíciles de detectar.

Los seis estados de un puerto, según *Nmap* son:

- ***Abierto***: una aplicación acepta conexiones TCP o paquetes UDP en este puerto.
- ***Cerrado***: un puerto cerrado es accesible ya que recibe y responde a las sondas de *Nmap*, pero no tiene una aplicación escuchando en él. Pueden ser útiles para determinar si un equipo está activo en cierta dirección IP y es parte del proceso de detección de sistema operativo.
- ***Filtrado***: *Nmap* no puede determinar si el puerto se encuentra abierto porque un filtrado de paquetes previene que sus sondas alcancen el puerto. Puede provenir un cortafuegos, de las reglas de un enrutador, o por una aplicación de cortafuegos instalada en el propio equipo. Estos puertos suelen frustrar a los atacantes, porque proporcionan muy poca información.
- ***No filtrado***: el puerto es accesible, pero *Nmap* no puede determinar si se encuentra abierto o cerrado. Solo el sondeo *ACK*, utilizado para determinar las reglas de un cortafuegos, clasifica a los puertos según este estado.
- ***Abierto|filtrado***: *Nmap* no puede determinar si el puerto se encuentra abierto o filtrado.
- ***Cerrado|filtrado***: *Nmap* no puede determinar si un puerto se encuentra cerrado o filtrado.


## Guía de uso

Las opciones de *Nmap* especificas para el escaneo de puertos son:

```bash
$ nmap -h
.
.
SCAN TECHNIQUES:
    -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
    -sU: UDP Scan
    -sN/sF/sX: TCP Null, FIN, and Xmas scans
    --scanflags <flags>: Customize TCP scan flags
    -sI <zombie host[:probeport]>: Idle scan
    -sY/sZ: SCTP INIT/COOKIE-ECHO scans
    -sO: IP protocol scan
    -b <FTP relay host>: FTP bounce scan
.
.
```

Para indicar los puertos sobre los que realizar el escaneo disponemos de las siguientes opciones:

```bash
$ nmap -h
PORT SPECIFICATION AND SCAN ORDER:
    -p <port ranges>: Only scan specified ports
     Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
    --exclude-ports <port ranges>: Exclude the specified ports from scanning
    -F: Fast mode - Scan fewer ports than the default scan
    -r: Scan ports sequentially - don't randomize
    --top-ports <number>: Scan <number> most common ports
    --port-ratio <ratio>: Scan ports more common than <ratio>
```

### Escaneo por defecto

Entendemos por escaneo por defecto aquel en el que no se especifíca ninguna opción de *Nmap*. En un escaneo por defecto, primero se ejecuta un descubrimiento de host (esto se hará a no ser que se especifique lo contrario) y después se envía un paquete *SYN* (*TCP Connect* si el usuario no tiene privilegios) al puerto indicado (a los 1000 más comunes si no es especifica ninguno). 

En caso de estar analizando Windows o un sistema que esté bloqueando el descubrimiento de hosts, es necesario incluir la opción *-PN*

```bash
$ nmap 192.168.10.54
```

### TCP SYN scan Vs. TCP Connect

Un escaneo *TCP SYN*, como su nombre indica, inicia un handshake tcp. Si se recibe *RESET* el puerto está cerrado, mientras que si recibe *ACK* está abierto. Este tipo de escaneo es más rápido y silencioso que *TCP Connect (-sT)* porque no completa el saludo, evitando que la conexión quede almacenada en los logs del sistema operativo. 

Por otro lado, aunque *TCP Connect* es más ruidoso, es preferible en ciertos contextos donde no es un problema que nos detecten los sistemas de defensa y necesitamos una mayor precisión. Por ejemplo, en un CTF o resolviendo un laboratorio HTB o similares.

```bash
# TCP SYN
$ nmap -sS 192.168.10.54

# TCP Connect
$ nmap -sT 192.168.10.54
```

### Detección de versiones y sistemas operativos

La detección de servicios y sus versiones es el siguiente paso después de encontrar los puertos abiertos en los sistemas objetivo. Esta informa entrará en juego durante la fase de análisis de vulnerabilidades, tratando de averiguar qué servicios están mal configurados, que servicios son débiles frente a una vulnerabilidad, etc.

```bash
$ nmap -h 
SERVICE/VERSION DETECTION:
    -sV: Probe open ports to determine service/version info
    --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
    --version-light: Limit to most likely probes (intensity 2)
    --version-all: Try every single probe (intensity 9)
    --version-trace: Show detailed version scan activity (for debugging)
OS DETECTION:
    -O: Enable OS detection
    --osscan-limit: Limit OS detection to promising targets
    --osscan-guess: Guess OS more aggressively

```

Con la opción *-sV* podemos obtener información sobre el servicio que está corriendo sobre cada puerto analizado, además de la versión concreta de cada servicio que obtengamos.

```bash
$ nmap -sS -p6421,41288,55413 -sV 192.140.84.3                                                                                                                     
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-01-27 01:55 IST
Nmap scan report for demo.ine.local (192.140.84.3)
Host is up (0.000042s latency).

PORT      STATE SERVICE VERSION
6421/tcp  open  mongodb MongoDB 2.6.10
41288/tcp open  achat   AChat chat system
55413/tcp open  ftp     vsftpd 3.0.3
MAC Address: 02:42:C0:8C:54:03 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.35 seconds

```

Para obtener información sobre el sistema operativo pordemos usar la opción *-O*. Esta opción puede combinarse con la detección de servicios, ya que puede ayudar a interpretar mejor el sistema operativo que está corriendo el objetivo.

```bash
$ nmap -sS -p- -sV -O 192.140.84.3                                                                                                                   
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-01-27 02:03 IST
Nmap scan report for demo.ine.local (192.140.84.3)
Host is up (0.000064s latency).
Not shown: 65532 closed tcp ports (reset)
PORT      STATE SERVICE VERSION
6421/tcp  open  mongodb MongoDB 2.6.10
41288/tcp open  achat   AChat chat system
55413/tcp open  ftp     vsftpd 3.0.3
MAC Address: 02:42:C0:8C:54:03 (Unknown)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.8
Network Distance: 1 hop
Service Info: OS: Unix

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.01 seconds
```

### Nmap Scripting Engine (NSE)

[⟵ Anterior](../03_activa.md#313-escaneo-de-puertos)
