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

Por ejemplo: supongamos que una empresa solicita un pentest, y se considera el siguiente bloque de direcciones: 200.200.0.0/16. Esta máscara de red significa que la red podría contener hasta 2¹⁶ - 2 hosts con direcciones IP en el rango 200.200.0.1 - 200.200.255.254. La primera tarea del pentester consiste en determinar cuáles de las direcciones IP están asignadas a un host, y cuáles de esos hosts están activos.

### 3.1.2. Descubrimiento de hosts



[](.md)

## 3.2. Enumeración

[⟵ Anterior](02_pasiva.md) | [Siguiente ⟶](.md)
