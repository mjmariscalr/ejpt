# Escaneo de puertos con Metasploit: aunxiliary modules

Los módulos auxiliares de Metasploit permiten realizar un escaneo de puertos y el descubrimiento de hosts. La principal ventaja de usar esta herramienta es que permite realizar un escaneo en la fase de post explotación, es decir, a una red local distinta desde un sistema objetivo tras realizar un pivoting.

## Guía de uso

El primer paso para trabajar con *MSF* es conocer los módulos de los que disponemos para cada tarea que vayamos a realizar. Para ello usamos la orden *search* acompañada de la clave a buscar. Por ejemplo:

```bash
msf > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/portscan/ftpbounce              .                normal  No     FTP Bounce Port Scanner
   1  auxiliary/scanner/natpmp/natpmp_portscan          .                normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/sap/sap_router_portscanner      .                normal  No     SAPRouter Port Scanner
   3  auxiliary/scanner/portscan/xmas                   .                normal  No     TCP "XMas" Port Scanner
   4  auxiliary/scanner/portscan/ack                    .                normal  No     TCP ACK Firewall Scanner
   5  auxiliary/scanner/portscan/tcp                    .                normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/syn                    .                normal  No     TCP SYN Port Scanner
   7  auxiliary/scanner/http/wordpress_pingback_access  .                normal  No     Wordpress Pingback Locator
```

Para interactuar con un módulo en especifico podemos hacerlo tanto con su indice, como con su nombre de la siguiente forma:

```bash
msf > use auxiliary/scanner/portscan/syn
msf auxiliary(scanner/portscan/syn) >
```

Con la siguiente orden podemos ver las opciones del modulo que estemos usando:

```bash
msf auxiliary(scanner/portscan/syn) > show options 

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds
```

Para modificar alguna de las opciones lo hacemos con el comando *set*, seguido del nombre de la opción y el valor que queramos asignar:

```bash
msf auxiliary(scanner/portscan/tcp) > set RHOST 192.168.1.1
RHOST => 192.168.1.1
```

Por último, para ejecutar el módulo auxiliar, simplemente lanzamos el comando *run*:

```bash
msf auxiliary(scanner/portscan/tcp) > run
[+] 192.168.1.1           - 192.168.1.1:80 - TCP OPEN
[+] 192.168.1.1           - 192.168.1.1:443 - TCP OPEN
[*] 192.168.1.1           - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

[⟵ Anterior](../03_activa.md#313-escaneo-de-puertos)
