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

A diferencia de otros escaneos, el escaneo ACK no intenta determinar si un puerto está abierto o cerrado. Su único objetivo es determinar si las respuestas están siendo manipuladas por un dispositivo intermedio (un firewall). *Nmap* envía un paquete con el flag ACK activado. Según las reglas del protocolo TCP, cualquier sistema que reciba un paquete ACK inesperado, sin una conexión previa establecida, debería responder con un paquete RST (Reset).

Interpretación según la respuesta:

- Unfiltered (No filtrado): Si el puerto devuelve un RST, *Nmap* lo marca como unfiltered. Esto significa que el paquete llegó al sistema operativo; por lo tanto, no hay un firewall bloqueándolo.
- Filtered (Filtrado): Si no hay respuesta (timeout) o si el firewall devuelve un error ICMP (tipo 3, código 1, 2, 3, 9, 10 o 13), *Nmap* lo marca como filtered. Esto confirma la presencia de un firewall.

## Fragmentación de paquetes (*-f*)

La fragmentación de paquetes tiene el objetivo de "engañar" a los dispositivos de seguridad dividiendo la cabecera TCP en trozos tan pequeños que al filtro le resulte difícil identificar qué tipo de escaneo se está realizando. *Nmap* divide la cabecera TCP de 8 bytes en fragmentos más pequeños. El primer fragmento suele contener solo una parte de la cabecera (como los puertos de origen y destino). El segundo fragmento contiene el resto (como los flags TCP).

Muchos firewalls antiguos o mal configurados inspeccionan los fragmentos IP de forma individual y no realizan un reensamblado completo antes del análisis. Al fragmentar un paquete TCP, la información necesaria para identificar un intento de conexión (cabeceras completas, flags TCP, puertos, etc.) puede no estar disponible en un solo fragmento, lo que provoca que el firewall no detecte correctamente el paquete y permita el paso del tráfico.

Variantes:

- ***-f* (Fragmentación estándar):** Divide el paquete en trozos de 8 bytes tras la cabecera IP.
- ***-ff* (Fragmentación severa):** Divide los paquetes en trozos de 16 bytes. Esto es más útil contra filtros que intentan ser un poco más inteligentes con los fragmentos de 8 bytes.
- ***--mtu* <valor>:** Te permite definir manualmente el tamaño máximo de la unidad de transmisión. El valor debe ser múltiplo de 8 (por ejemplo, --mtu 24).

## Opción *--data-length*

La opción --data-length de *Nmap* permite modificar el tamaño de los paquetes enviados durante un escaneo añadiendo datos aleatorios al payload. Esta técnica no altera las cabeceras IP ni TCP/UDP, por lo que el funcionamiento del escaneo se mantiene intacto, pero su huella de red cambia.

Al incrementar el tamaño de los paquetes, el tráfico generado deja de coincidir con los patrones típicos de *Nmap*. Esto puede ayudar a evadir sistemas de detección que se basan en firmas simples, como tamaños de paquete fijos o secuencias previsibles. El host objetivo ignora estos datos adicionales, ya que no afectan al establecimiento de la conexión ni al procesamiento del paquete.

## Suplantación de direcciones (*-D*)

La opción -D (Decoy) de *Nmap* se utiliza para ocultar la dirección IP real del escáner mezclándola con IPs señuelo (decoys) durante un escaneo. Para usar esta opción es necesario estar conectado a la red local del equipo que queremos escanear.

Cuando se usa -D, *Nmap* envía paquetes al objetivo desde múltiples direcciones IP aparentemente distintas. Solo una de ellas es la IP real del atacante; las demás son señuelos falsos.

Ejemplo:

```bash
$ nmap -sS -D 192.168.1.5,192.168.1.8 192.168.1.10
```

Esta técnica no evita la detección del escaneo, solo dificulta identificar su origen real.

## Plantilla de tiempo (*-T[0-5]*)

La opción -T de *Nmap* controla  cómo *Nmap* gestiona los tiempos de espera, retrasos entre paquetes y paralelismo. Ajustar este parámetro permite priorizar velocidad o sigilo. *Nmap* define seis valores, numerados del 0 al 5, donde valores bajos generan escaneos más lentos y discretos, y valores altos priorizan la rapidez a costa de una mayor probabilidad de detección:

- ***-T0* (Paranoid):** diseñado para entornos altamente restrictivos. Envía paquetes con retrasos muy largos entre ellos, minimizando la posibilidad de activar sistemas de detección basados en frecuencia. Es extremadamente lento y se usa casi exclusivamente con fines académicos o en pruebas muy específicas.
- ***-T1* (Sneaky):** reduce ligeramente los retrasos respecto a -T0, pero sigue siendo muy conservador. Puede evadir algunos IDS simples basados en umbrales de tiempo, aunque el escaneo sigue siendo lento.
- ***-T2* (Polite):** introduce pausas deliberadas para reducir la carga sobre la red y el objetivo. No está orientado a evasión, sino a escaneos cuidadosos en sistemas sensibles o entornos compartidos.
- ***-T3* (Normal):** es el valor por defecto. Ofrece un equilibrio entre velocidad y fiabilidad, y es adecuado para la mayoría de los escaneos estándar.
- ***-T4* (Aggressive):** acelera el escaneo aumentando el paralelismo y reduciendo los tiempos de espera. Está pensado para redes rápidas y estables, como entornos locales o auditorías controladas. Incrementa significativamente la probabilidad de ser detectados.
- ***-T5* (Insane):** maximiza la velocidad al mínimo coste de fiabilidad y sigilo. Reduce drásticamente los timeouts y asume una red muy rápida y sin pérdidas. Puede generar falsos negativos y es fácilmente detectable por IDS/IPS.

## Opción *--scan-delay*

La opción --scan-delay de *Nmap* permite introducir un retraso fijo entre el envío de paquetes durante un escaneo. Con esta opción, *Nmap* espera el tiempo indicado antes de enviar el siguiente paquete, independientemente del estado de la red o de la velocidad de respuesta del host. Esto reduce la frecuencia del tráfico generado y puede ayudar a evitar sistemas de detección que se basan en un número elevado de paquetes en un corto periodo de tiempo.


[⟵ Anterior](../03_activa.md#descubrimiento-de-hosts)
