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

De la misma forma que la fase anterior, la principal herramienta para esta fase es [Nmap](nmap/port_scanning.md).

## 3.2. Enumeración

[⟵ Anterior](02_pasiva.md) | [Siguiente ⟶](.md)
