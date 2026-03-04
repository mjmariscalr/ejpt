# 3. Recolección de información activa

Después de recoger información en la fase de reconocimiento pasivo nos moveremos a la fase de reconocimiento activo. Esta fase, a diferencia del reconocimiento pasivo donde se recolecta información de fuentes públicas sin alertar al sistema, implica el envío de paquetes de datos y solicitudes específicas para provocar una respuesta de los dispositivos y servicios en red.

El objetivo principal de esta etapa es trazar un mapa detallado de la superficie de ataque mediante la identificación de:

- Dispositivos activos.
- Servicios y puertos.
- Estructura interna de la red.
- Sistemas operativos.
- Enumeración de la información: usuarios, servicios, directorios, etc.

Debido a la interacción directa con el objetivo, esta fase es detectable. Una recolección agresiva puede alertar a los sistemas de defensa, pero si es demasiado cautelosa podría omitir información crítica.

## 3.1. Scanning

El scanning o escaneo de redes o hosts es la primera fase donde empezamos a interactuar directamente con el objetivo para descubrir dispositivos encendidos, servicios en ejecución o puertos abiertos. El objetivo principal de esta fase es construir un mapa de la superficie de ataque. Esta fase implica: descubrimiento de hosts, escaneo de puertos y enumeración.

Para determinar que hosts están activos en la red, que puertos se encuentran abiertos en un dispositivo o que sistema operativo está usando se usa el *network mapping*. *Network mapping* se refiere al proceso de descubrir e identificar dispositivos y elementos de la infraestructura de red.

### 3.1.2. Descubrimiento de hosts

El descubrimiento de hosts es una fase crucial en un pentest, ya que permite identificar equipos activos en la red antes de hacer una exploración más profunda. La elección de la técnica a usar depende de factores como: las características de la red, cómo de sigilosos debemos ser o los objetivos del pentest.
 
Por ejemplo: supongamos que una empresa solicita un pentest, y se considera el siguiente bloque de direcciones: 200.200.0.0/16. Esta máscara de red significa que la red podría contener hasta 2¹⁶ - 2 hosts con direcciones IP en el rango 200.200.0.1 - 200.200.255.254. La primera tarea del pentester consiste en determinar cuáles de las direcciones IP están asignadas a un host, y cuáles de esos hosts están activos.

Algunas de las técnicas más importantes son:

- **Ping sweep:** envía peticiones *ICMP* (ping) a un rango de direcciones. 
	- Ventajas: rápido y ampliamente soportado.
	- Inconvenientes: algunos equipos o firewalls pueden bloquear el tráfico *ICMP*.
- **ARP scanning:** solo funciona en redes locales, es decir, es necesario estar conectado a la red.
- **TCP SYN ping:** envía peticiones *TCP SYN* a un puerto específico y si el objetivo está activo devuelve un paquete *TCP SYN-ACK*.
	- Ventajas: es más sigiloso que *ICMP* y puede evitar ciertas defensas.
	- Inconvenientes: algunos equipos pueden no responder a peticiones *TCP SYN*.
- **UDP ping:** se envían paquetes *UDP* a un puerto específico. Puede ser útil para hosts que no responden a *ICMP* o *TCP*.
- **TCP ACK ping:** envía un paquete *TCP SYN* a un puerto específico. Si se recibe un paquete *TCP RST*, significa que el equipo está activo.
- **SYN-ACK ping:** envía un paquete *TCP SYN-ACK* a un puerto específico. Si se recibe un paquete *TCP RST*, significa que el equipo está activo.

Para el [descubrimiento de hosts](nmap/host_discovery.md) y el resto de tareas relacionadas con el escaneo de redes y equipos usaremos nmap.

### 3.1.3. Escaneo de puertos

La siguiente fase lógica de un pentesting es intentar obtener más información a partir de todo lo que hemos encontrado en la fase de descubrimiento de hosts. Esta fase consiste en ser más específicos en cada uno de los objetivos encontrados para obtener información sobre ellos. Para ello, empezamos por el descubrimiento de puertos, y posteriormente podremos explorar más detalles.

De la misma forma que la fase anterior, la principal herramienta para esta fase es [Nmap](nmap/port_scanning.md), aunque también podemos usar los [módulos auxiliares](msf/escaneo.md) de *metasploit* que se explican en la sección [3.2.2](#322-msf-módulos-auxiliares).

### 3.1.4. Detección de firewall y evasión de IDS

En un escenario de laboratorio, los escaneos de Nmap devuelven resultados claros y directos. Sin embargo, en un entorno de real, el pentester se enfrenta a un adversario silencioso: los sistemas de defensa perimetral. Los Firewalls y los Sistemas de Detección de Intrusos (IDS) están diseñados precisamente para identificar y bloquear el ruido generado por un escaneo de puertos convencional.

La guía de uso de *Nmap* para este propósito se encuentra [aquí](nmap/evasion.md).

## 3.2. Enumeración

El objetivo de la enumeración es obtener información más detallada sobre los sistemas de una red y los servicios activos en ellos. Esto incluye información como: nombres de cuentas, archivos compartidos y servicios mal configurados. Igual que el escaneo de puertos, la enumeración implica conexiones activas a los distintos dispositivos objetivo.

### 3.2.1. *Nmap* Scripting Engine (NSE)

Uno de los motivos que hacen a *Nmap* una herramienta tan potente es el hecho de que, además de escaneo de puertos y servicios, también permite realizar enumeración de los mismos. Otra de las ventajas que tiene es que nos permite exportar los resultados con un formato legible por *Metasploit*, para posteriormente poder realizar un análisis de vulnerabilidades y explotación.

Para la enumeración con *nmap* tenemos varias opciones en función del objetivo:

- *-sV*: muestra las versiones de los servicios que corren en los puertos abiertos.
- *-sC*: lanza los scripts por defecto para obtener más información de los servicios. Es equivalente a *--script=default*.
- *--script*: permite elegir scripts especificos para un determinado servicio, que analizan más en profundidad que los scripts por defecto. Podemos encontrar los scripts en: */usr/share/nmap/scripts*.

### 3.2.2. MSF: módulos auxiliares

Los módulos auxiliares se usan para realizar tareas como: escaneo, descubrimiento y fuzzing. Permiten realizar escaneo TCP y UDP, además de enumerar distintos servicios. Una de sus principales ventajas es que se pueden usar tanto en la fase de reconocimiento como en la de post explotación donde, por ejemplo, podemos realizar un escaneo en una red diferente después de conseguir acceso a un sistema objetivo.

Algunos de los servicios que se pueden enumerar a través de los módulos auxiliares de *Metasploit* son:

- [FTP](msf/ftp_enum.md)
- [SMB](msf/.md)
- [Web server](msf/.md)
- [MySQL](msf/.md)
- [SSH](msf/.md)
- [SMTP](msf/.md)

[⟵ Anterior](02_pasiva.md) | [Siguiente ⟶](.md)
