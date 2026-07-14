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
