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
