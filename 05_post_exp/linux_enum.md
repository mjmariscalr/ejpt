# 2. Enumeración Linux

La fase de enumeración permite identificar las características del sistema operativo, su configuración y los componentes instalados, información que servirá como base para las siguientes etapas de la post-explotación. Conocer estos aspectos ayuda a comprender mejor el objetivo y permite determinar qué técnicas de escalada de privilegios, persistencia o movimiento lateral pueden ser viables.

## Enumeración del sistema

Después de obtener acceso inicial a un sistema objetivo, siempre es importante recopilar más información sobre él, como qué sistema operativo está ejecutando y cuál es su versión. Esta información es muy útil, ya que nos da una idea de lo que podemos hacer y del tipo de exploits que podrían ser compatibles.

Buscamos:

- Nombre del equipo.
- Distribución del sistema operativo y su versión.
- Versión del kernel y arquitectura.
- Información de la CPU.
- Información de los discos y unidades montadas.
- Paquetes o software instalados.

**Información del sistema:**

```bash
meterpreter > sysinfo
Computer        : 192.168.10.25
OS              : Debian 9.5 (Linux 5.4)
Architecture    : x64
Meterpreter     : x86/linux
```

**Obtener una sesion bash interactiva desde meterpreter:**

```bash
meterpreter > shell
/bin/bash -i
usr@hostname:
```

**Hostname:**

Al obtener una shell interaciva con el paso anterior podemos ver el nombre del equipo, pero tambien podemos hacerlo con el comando hostname:

```bash
usr@hostname: hostname
```

**Comprobar la distribución:**

```bash
usr@hostname: cat /etc/*release
```

**Información del sistema:**

```bash
usr@hostname: uname -a                                                                        
Linux hostname 5.14.28-1-generic #1 SMP PREEMPT_DYNAMIC Sat, 04 Jul 2026 20:51:33 +0000 x86_64 GNU/Linux
```
