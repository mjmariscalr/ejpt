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
