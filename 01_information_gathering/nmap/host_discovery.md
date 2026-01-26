# Descubrimiento de hosts con *Nmap*

*Nmap* (*Network Mapper*) es una herramienta de código abierto indispensable para la seguridad informática y la administración de redes. Se utiliza principalmente para descubrir dispositivos en una red y analizar su seguridad.

*Nmap* envía paquetes de datos a un objetivo y luego analiza las respuestas para determinar qué está ocurriendo. Si el sistema objetivo envía una respuesta, en el caso concreto del descubrimiento de hosts, el sistema está activo. Si no hay respuesta, está inactivo o filtrado por un firewall. 

## Guía de uso

Aunque *Nmap* es flexible en cuanto a sintaxis, el formato habitual es el siguiente:

```bash
$ nmap opciones objetivo
```

Es importante tener en cuenta el nivel de privilegios que tenemos a la hora de usar *Nmap*, ya que para usar algunas de las utilidades de las que dispone es necesario tener privilegios *sudo* o *root*.

Las opciones de *Nmap* especificas para el descubrimiento de hosts son:

```bash
$ nmap -h
.
.
HOST DISCOVERY:
    -sL: List Scan - simply list targets to scan
    -sn: Ping Scan - disable port scan
    -Pn: Treat all hosts as online -- skip host discovery
    -PS/PA/PU/PY[portlist]: TCP SYN, TCP ACK, UDP or SCTP discovery to given ports
    -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
    -PO[protocol list]: IP Protocol Ping
    -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
    --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
    --system-dns: Use OS's DNS resolver
    --traceroute: Trace hop path to each host
.
.
```

### Ping Scan o No port scan (*-sn*)

Por defecto, el descubrimiento de hosts con *-sn* consiste en consultas *ICMP echo*, *TCP SYN* al puerto 443, *TCP ACK* al puerto 80 y peticiones *ICMP timestamp*. En caso de no tener privilegios, solo se envían paquetes *SYN* a los puertos 80 y 443. Si esta opción se usa dentro de una red local solo se envían consultas *ARP*, a no se que se especifique *--send-ip*. Además, se puede combinar con todas las las opciones -P* excepto -Pn.

### Formato de IPs

Nmap admite distintos formatos a la hora de especificar el objetivo en función de lo que queremos escanear:

- **IP unica:** analiza un único objetivo (*ej: 10.0.3.48*).
- **Rango de IPs:** analiza un conjunto de direcciones dentro de la red (*ej: 10.4.23.225-247*).
- **IPs separadas por un espacio:** igual que con una única dirección, pero con varias.
- **Conjunto de IPs de una red:** nmap permite especificar varias direcciones dentro de una red sin escribir la dirección completa (*ej: 10.0.3.48,52,54,55,74*).
- **Red con mascara:** en este caso analiza la red completa, es especialmente útil en la fase de descubrimiento de hosts.
- **Fichero:** en un archivo se añade lista que puede tener todos formatos anteriores (*ej: nmap -sn -iL direcciones.txt*).

### *TCP ACK* (*-PA*)

En el caso de realizar un escaneo con paquetes *TCP ACK*, es posible que tengamos problemas porque algunos sistemas operativos o firewalls puede estar configurados para bloquear este tipo de paquetes. 

[⟵ Anterior](../03_activa.md#descubrimiento-de-hosts)
