# Detección de firewall y evasión de IDS: Guía de uso

El éxito de una exploración depende de la manipulación precisa de los paquetes para evitar disparar alertas de seguridad o bloqueos automáticos.

Objetivos:

- **Detección de Firewalls:** Identificar puertos filtrados mediante paquetes *TCP ACK*.
- **Técnicas de Evasión:** implementar fragmentación de paquetes, suplantación de direcciones (decoys) y manipulación de MTU.
- **Gestión de Tiempos :** Ajustar la agresividad del escaneo para evitar umbrales de detección.

Las opciones de *Nmap* son:

```bash
$ nmap -h
TIMING AND PERFORMANCE:
    Options which take <time> are in seconds, or append 'ms' (milliseconds),
    's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
    -T<0-5>: Set timing template (higher is faster)
    --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
    --min-parallelism/max-parallelism <numprobes>: Probe parallelization
    --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
    --max-retries <tries>: Caps number of port scan probe retransmissions.
    --host-timeout <time>: Give up on target after this long
    --scan-delay/--max-scan-delay <time>: Adjust delay between probes
    --min-rate <number>: Send packets no slower than <number> per second
    --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
    -f; --mtu <val>: fragment packets (optionally w/given MTU)
    -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
    -S <IP_Address>: Spoof source address
    -e <iface>: Use specified interface
    -g/--source-port <portnum>: Use given port number
    --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
    --data <hex string>: Append a custom payload to sent packets
    --data-string <string>: Append a custom ASCII string to sent packets
    --data-length <num>: Append random data to sent packets
    --ip-options <options>: Send packets with specified ip options
    --ttl <val>: Set IP time-to-live field
    --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
    --badsum: Send packets with a bogus TCP/UDP/SCTP checksum

```

## Detección de firewall (*-sA*)

A diferencia de otros escaneos, el escaneo ACK no intenta determinar si un puerto está abierto o cerrado. Su único objetivo es determinar si las respuestas están siendo manipuladas por un dispositivo intermedio (un firewall). Nmap envía un paquete con el flag ACK activado. Según las reglas del protocolo TCP, cualquier sistema que reciba un paquete ACK inesperado, sin una conexión previa establecida, debería responder con un paquete RST (Reset).

Interpretación según la respuesta:

- Unfiltered (No filtrado): Si el puerto devuelve un RST, Nmap lo marca como unfiltered. Esto significa que el paquete llegó al sistema operativo; por lo tanto, no hay un firewall bloqueándolo.
- Filtered (Filtrado): Si no hay respuesta (timeout) o si el firewall devuelve un error ICMP (tipo 3, código 1, 2, 3, 9, 10 o 13), Nmap lo marca como filtered. Esto confirma la presencia de un firewall.

## Fragmentación de paquetes (*-f*)



## Opción *--data-length*



## Suplantación de direcciones (*-D*)



## Plantilla de tiempo (*-T[0-5]*)



## Opción *--scan-delay*




[⟵ Anterior](../03_activa.md#descubrimiento-de-hosts)
