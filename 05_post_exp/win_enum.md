# 2. Enumeración Windows

La fase de enumeración permite identificar las características del sistema operativo, su configuración y los componentes instalados, información que servirá como base para las siguientes etapas de la post-explotación. Conocer estos aspectos ayuda a comprender mejor el objetivo y permite determinar qué técnicas de escalada de privilegios, persistencia o movimiento lateral pueden ser viables.

## Enumeración del sistema

Después de obtener acceso inicial a un sistema objetivo, siempre es importante recopilar más información sobre él, como qué sistema operativo está ejecutando y cuál es su versión. Esta información es muy útil, ya que nos da una idea de lo que podemos hacer y del tipo de exploits que podemos utilizar. Buscamos:

- Nombre del equipo (Hostname).
- Nombre del sistema operativo (Windows 7, Windows 8, etc.).
- Compilación del sistema operativo y Service Pack (por ejemplo, Windows 7 SP1, compilación 7600).
- Arquitectura del sistema operativo (x64 o x86).
- Actualizaciones y correcciones de seguridad instaladas (Installed updates/Hotfixes).

**Nombre del equipo y usuario desde meterpreter:**

```bash
meterpreter > getuid
Server username: WIN-OMCNBKR66MN\Administrator
```

**Información del sistema:**

```bash
meterpreter > sysinfo
Computer        : WIN-OMCNBKR66MN
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows
```

**Toda la información del sistema:**

```bash
meterpreter > shell # Abre una consola de comandos nativa del sistema.
C:\hfs> systeminfo  # Muestra toda la información del sistema, incluyendo actualizaciones y correcciones de seguridad.
```

**Información adicional sobre las actualizaciones:**

```bash
C:\hfs> wmic qfe get Caption,Description,HotFixID,InstalledOn
```

**Versión del sistema:**

```bash
meterpreter > cat C:\Windows\System32\eula.txt # No está disponible en todas las versiones de Windows
```

## Enumeración de grupos y usuarios

Es importante recopilar más información sobre las cuentas de usuario del sistema, como el usuario al que se tiene acceso y las demás cuentas de usuario existentes. Buscamos:

- Usuario actual y sus privilegios.
- Información adicional sobre las cuentas de usuario.
- Otros usuarios presentes en el sistema.
- Grupos de usuarios.
- Miembros del grupo integrado de administradores (Built-in Administrators).

**Enumerar los privilegios asociados al usuario actual:**

El comando `getprivs` de Meterpreter tiene la finalidad de identificar qué privilegios del sistema están disponibles para el usuario comprometido, lo que permite evaluar las capacidades de la sesión y determinar si es posible realizar determinadas acciones o aplicar técnicas de escalada de privilegios que dependan de dichos privilegios.

```bash
meterpreter > getprivs
```

**Enumerar los usuarios del sistema con msfconsole:**

El módulo `enum_logged_on_users` recopila información sobre las cuentas de usuario que tienen o han tenido sesiones iniciadas en el sistema comprometido de forma local, remota o mediante otros mecanismos compatibles, detectando posibles cuentas con privilegios elevados.

```bash
msf > use post/windows/gather/enum_logged_on_users
msf post(windows/gather/enum_logged_on_users) > set session <id>
msf post(windows/gather/enum_logged_on_users) > run
```

**Enumerar los usuarios del sistema de forma manual:**

El comando `query user` es una utilidad nativa de Windows que muestra información sobre las sesiones de usuario existentes en el sistema. Permite identificar qué usuarios tienen sesiones activas, desconectadas o en espera, así como obtener información relacionada con el estado de cada sesión.

```bash
meterpreter > shell
C:\ > query user
```

El comando `net user` es una utilidad nativa de Windows utilizada para consultar y administrar cuentas de usuario locales o de dominio según el contexto. Permite obtener información sobre las cuentas existentes en el sistema, la pertenencia a grupos, el estado de la cuenta, la fecha del último inicio de sesión o la configuración relacionada con la contraseña.

```bash
meterpreter > shell
C:\ > net user
C:\ > net user <usuario> # Muestra información del usuario, como cuando debe cambiar la contraseña.
```

**Grupos locales:**

```bash
meterpreter > shell
C:\ > net localgroup
C:\ > net localgroup <grupo> # Muestra los miembros del grupo.
```

## Información de red


**Dirección IP actual, adaptador de red, máscara de red y puerta de enlace.**

```bash
C:\ > ipconfig
C:\ > ipconfig \all # Proporciona información adicional como: dirección MAC o información de activación sobre DHCP.
```

**Servicios TCP/UDP en ejecución y sus respectivos puertos.**

Podemos ver todas las conexiones de red y los puertos abiertos del equipo, junto con el proceso que los está utilizando. Estados más comunes:

- **LISTENING:** el programa está esperando conexiones entrantes.
- **ESTABLISHED:** hay una conexión activa entre dos equipos.
- **TIME_WAIT:** la conexión se cerró recientemente y el sistema espera antes de liberar el puerto.
- **CLOSE_WAIT:** el equipo remoto cerró la conexión y la aplicación local aún no la ha cerrado.
- **SYN_SENT:** se ha iniciado una conexión y se espera respuesta.
- **SYN_RECEIVED:** se recibió una solicitud de conexión y está en proceso de establecerse.

```bash
C:\ > netstat -ano
```

**Tabla de enrutamiento.**

```bash
C:\ > route print
```

**Tabla ARP.**

Muestra la tabla ARP (Address Resolution Protocol) del equipo. Contiene los dispositivos con los que ha tenido comunicación recientemente dentro de la misma red local.

```bash
C:\ > arp -a
```

**Estado del Firewall de Windows.**

En algunos casos el primer comando puede estar discontinuado.

```bash
C:\ > netsh firewall show state
C:\ > netsh advfirewall firewall show
```

## Procesos y servicios

Un proceso es una instancia de un archivo ejecutable (.exe) o de un programa que está en ejecución.
Un servicio es un proceso que se ejecuta en segundo plano y no interactúa con el escritorio del usuario.

### Procesos con meterpreter.

En el listado que obtenemos con este comando se muestra, entre otra información, el PID, nombre del ejecutable y usuario

```bash
meterpreter > ps
```

Para ver un proceso en concreto podemos usar `pgrep <proceso>`. Este comando es útil para migrar a una sesión más estable, como puede ser el proceso `explorer.exe`. Para migrar usamos `migrate <pid>`

### Servicios con una shell estándar.

- `net start`: Muestra todos los servicios de Windows que están actualmente en ejecución.
- `wmic service list brief`: Presenta un listado resumido de todos los servicios del sistema, incluyendo información como el nombre del servicio, su estado (en ejecución o detenido) y el modo de inicio.
- `tasklist /SVC`: Muestra los procesos en ejecución junto con los servicios asociados a cada proceso. Permite identificar qué servicios se están ejecutando dentro de un mismo proceso.
- `schtasks /query /fo LIST:` Muestra un listado detallado de las tareas programadas en el sistema utilizando un formato de lista. Para cada tarea se incluye información como el nombre, el estado, la fecha y hora de la próxima ejecución, la última vez que se ejecutó y el usuario bajo el que se ejecuta. Esta información será útil durante la escalada de privilegios.

**Tareas programadas.**

```bash
C:\ > 
```

## Automatización

Además de realizar la enumeración local de forma manual, también podemos automatizar el proceso con la ayuda de algunos scripts y módulos de MSF. Aunque es importante conocer las técnicas y comandos de enumeración local, es necesario trabajar de manera eficiente en términos de tiempo.

Además de automatizar la recopilación de información como los datos del sistema, los usuarios y grupos, etc., estos scripts proporcionan información adicional sobre el sistema objetivo, como:

- Vulnerabilidades de escalada de privilegios.
- Contraseñas almacenadas localmente.
- Otra información útil para la evaluación de seguridad del sistema.
