# 1. Introducción a *Metasploit Framework*

*Metasploit Framework* es una herramienta de pentesting robusta y de código abierto usada por pentesters e investigadores que permite automatizar cada fase del ciclo de vida de un test de intrusión y proporciona un entorno para desarrollar exploits, siendo una de las bases de datos de exploits públicos y testeados más grande.

Terminología:

- **Módulo:** fragmentos de código que realizan una determinada tarea.
- **Vulnerabilidad:** debilidades en un equipo o red que puede ser explotada.
- **Exploit:** módulo que puede tomar ventaja de una vulnerabilidad.
- **Payload:** fragmento de código enviado a un objetivo para ejecutar código arbitrario o proporcionar acceso remoto.
- **Listener:** utilidad que espera una conexión desde el objetivo.

# 2. Arquitectura de *Metasploit Framework*

*MSF* consta principalmente de tres componentes: 

- **Interfaces:** forma de interactuar con metasploit. Las principales son MSFconsole, MSFcli, Armitage y la interfaz web.
- **Librerías:** facilitan la ejecución de los módulos. 
- **Módulos:** fragmento de código que se puede usar en *MSF*. Podemos encontrar los siguientes tipos:
	- **Exploit:** se usa para aprovechar una vulnerabilidad y se combina con un payload.
	- **Payload:** código entregado por *MSF* y ejecutado remotamente en el objetivo después de una explotación. Por ejemplo: una shell inversa que inicia una conexión desde el sistema objetivo.
	- **NOPS:** aseguran que los tamaños de los payloads sean consistentes y garantizan su estabilidad cuando se ejecutan.
	- **Auxiliary:** realiza funcionalidades adicionales como escaneo de puertos y enumeración.

*MSF* proporciona dos tipos de payloads que pueden combinarse con un exploit:

- Payload no escalonado (Non-Staged Payload): se envía al sistema objetivo tal cual, junto con el exploit.
- Payload escalonado (Staged Payload): se envía al objetivo en dos partes, la primera (stager) establece una conexión inversa hacia el atacante, descarga la segunda parte del payload (stage) y la ejecuta.

# 3. Pentesting con *MSF*

MSF puede usarse para realizar y automatizar tareas que forman parte del ciclo de vida de una prueba de penetración.

| Fase de Pruebas de Penetración | Implementación del Framework Metasploit |
|---|---|
| Recopilación de Información y Enumeración | Módulos Auxiliares |
| Escaneo de Vulnerabilidades | Módulos Auxiliares <br> Nessus |
| Explotación | Módulos de Exploit y Payloads |
| Post-Explotación | Meterpreter |
| Escalada de Privilegios | Módulos de Post-Explotación <br> Meterpreter |
| Mantenimiento de Acceso Persistente | Módulos de Post-Explotación <br> Módulos de Persistencia |

# 4. Configuración inicial

Iniciar el servicio postgresql:

```bash
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

Inicialización de la base de datos en *metasploit*:

```bash
msfdb init --connection-string=postgresql://postgres@localhost:5432/postgres
```

# 5. *MSFconsole*


Variables:

| Variable | Propósito |
|---|---|
| LHOST | almacena la dirección IP del sistema del atacante. |
| LPORT | almacena el número de puerto del sistema del atacante que se usará para recibir una conexión inversa. |
| RHOST | almacena la dirección IP del sistema/servidor objetivo. |
| RHOSTS | especifica las direcciones IP de múltiples sistemas objetivo o rangos de red. |
| RPORT | almacena el número de puerto que estamos atacando en el sistema objetivo. |
