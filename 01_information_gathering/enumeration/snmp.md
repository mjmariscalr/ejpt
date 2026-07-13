# Protocolo SNMP

**SNMP (Simple Network Management Protocol)** es un protocolo utilizado para supervisar y administrar dispositivos conectados a una red, como routers, switches, impresoras, servidores y otros equipos. Permite consultar el estado de los dispositivos, configurar determinados parámetros y recibir alertas cuando se producen eventos específicos.

### Funcionamiento de SNMP

SNMP es un **protocolo de la capa de aplicación** que normalmente utiliza **UDP** como protocolo de transporte. Está compuesto por tres elementos principales:

- **SNMP Manager (Administrador SNMP):** Sistema encargado de consultar e interactuar con los agentes SNMP de los dispositivos de la red.
- **SNMP Agent (Agente SNMP):** Software que se ejecuta en los dispositivos de red y que responde a las consultas SNMP y envía notificaciones.
- **Management Information Base (MIB):** Base de datos jerárquica que define la estructura de la información disponible a través de SNMP. Cada dato se identifica mediante un **Object Identifier (OID)** único.

### Versiones de SNMP

* **SNMPv1:** Primera versión. Utiliza **cadenas de comunidad (community strings)**, que funcionan básicamente como contraseñas para la autenticación.
* **SNMPv2c:** Versión mejorada que incorpora soporte para transferencias masivas de datos, aunque sigue utilizando cadenas de comunidad para la autenticación.
* **SNMPv3:** Introduce importantes mejoras de seguridad, como **cifrado**, **integridad de los mensajes** y **autenticación basada en usuarios**.

### Puertos

* **Puerto 161 (UDP):** Utilizado para las consultas SNMP.
* **Puerto 162 (UDP):** Utilizado para los **SNMP traps** o notificaciones enviadas por los dispositivos.

## Enumeración

La **enumeración SNMP** consiste en consultar dispositivos con **SNMP** habilitado para recopilar información útil que permita identificar posibles vulnerabilidades, configuraciones incorrectas o puntos de ataque.

Objetivos de la enumeración SNMP:

- Identificar dispositivos con SNMP habilitado y comprobar si son vulnerables.
- Extraer información del sistema.
- Identificar las cadenas de comunidad, ya que podrían permitir el acceso.
- Obtener configuraciones de red: tablas de enrutamiento, interfaces de red, direcciones IP, etc.
- Recopilar información de usuarios y grupos.
- Identificar servicios y aplicaciones: Determinar qué servicios y aplicaciones se están ejecutando en los dispositivos objetivo, lo que puede facilitar la identificación de nuevos vectores de ataque.

**Comprobar si está activo:**

```bash
nmap -sU -p 161,162 host
```

**Identificar community strings:**

```bash
nmap -sU --script snmp-brute -p 161,162 host 
```

**Extraer información con snpwalk:**

Es una herramienta que permite enumerar la información disponible en un dispositivo con SNMP habilitado. Consulta y recorre los OID de la MIB para obtener datos como el nombre del equipo, interfaces de red, sistema operativo, configuración y otros detalles del dispositivo.

```bash
snmpwalk -v 1 -c <comm_string> <host>
```

Podemos hacer algo similar, pero en un formato más legible usando `nmap`:

```bash
nmap -sU --script snmp-* -p 161,162 host 
```
